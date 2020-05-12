## Stateless Observable-based services

Create a new file for you service, in this example I'm using `users`, so create a `users/users.service.ts`

```ts
import { Injectable } from "@angular/core";
import { HttpClient } from "@angular/common/http";

@Injectable({
  providedIn: "root",
})
export class UsersService {
  constructor(private http$: HttpClient) {}

  loadAllUsers(): Observable<User[]> {
    return this.http$
      .get<User[]>("/apis/users")
      .pipe(map((response) => res["payload"]);
  }
}
```

## Consuming Observable-bases services using Angular async pipe

In your component you can implement the following pattern

```ts
@Component({})
export class HomeComponent implements OnInit {
  activeUsers$: Observables<User[]>;

  constructor(coursesService: UsersServices) {}

  ngOnInit(): void {
    const users$ = this.usersService.loadAllCourses();

    this.activeUsers$ = users$.pipe(
      map((users) => courses.filter((user) => user.active))
    );
  }
}
```

In the template you should in implement the following logic.

When we use async pipe it unsubscribe the observable automatically when destroyed, preventing memory leaks.

```html
<ng-container *ngIf="let user of (activeUsers$ | async)"></ng-container>
```
