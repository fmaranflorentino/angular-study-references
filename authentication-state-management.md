## Authentication State Management - Simple Reactive approach

Here we are going to develop a simple authentication flow

We need to create a `auth.store.ts` file, to handle login funcionality

```ts
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class AuthStoreService {
  private subject = new BehaviorSubject<User>(null);

  user$: Observable<User>;
  isLoggedIn$: Observable<boolean>;
  isLoggedOut$: Observable<boolean>;

  constructor(private http: HttpClient) {
    this.isLoggedIn$ = this.user$.pipe(map((user) => !!user));
    this.isLoggedOut$ = this.isLoggedIn$.pipe(map((user) => !user));

    const user = localStorage.getItem('auth_data');

    if (user) {
      this.subject.next(JSON.parse(user));
    }
  }

  login({ email: string, password: string }): Observable<User> {
    this.http
      .post<User>("/api/login", { email, password })
      .pipe(
        tap((user) => {
          this.subject.next(user);
          localStorage.setItem('auth_data', JSON.stringify(user));
        }),
        shareReplay()
      );
  }

  logout() {
    this.subject.next(null);
    localStorage.removeItem('auth_data');

    // navigate to home with router
  }
}
```

Now let's see how to consume this service in the `login.component.ts`

```ts
@Component({})
export class YourComponent {
  constructor(private auth: AuthStoreService);

  login({ email: string, password: string }) {
    this.auth.login({ email, password }).subscribe(
      () => {
        // here you can navigate between pages as your need
      },
      (err) => alert(err)
    );
  }

  logout() {
    this.auth.logout();
  }
}
```

Implementing the change on a view

```ts
export class YourComponent {
  constructor(public auth: authStoreService) {}
}
```

```html
  <button *ngIf="auth.isLoggedIn$ | async">LogOut</button>
```
