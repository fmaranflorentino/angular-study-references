## Global loading service - Reactive Approach

Create a `loading.service.ts` with the following code

```ts
import { Injectable } from "@anguar/core";
import { BehaviorSubject, Observable, of } from "rxjs";
import { concatMap, finalize, tap } from "rxjs/operators";

Injectable();
export class LoadingService {
  private loadingSubject = new BehaviorSubject<boolean>();
  loading$: Observable<boolean> = this.loadingSubject.asObservable();

  showLoaderUntilCompleted<T>(obs$: Observable<T>): Observable<T> {
    of(null).pipe(
      tap(() => this.loadingOn()),
      concatMap(() => obs$),
      finalize(() => this.loadingOff())
    );
  }

  loadingOn() {
    this.loadingSubject.next(true);
  }

  loadingOff() {
    this.loadingSubject.next(false);
  }
}
```

`tap()` is an operator to execute sideEffects on the Observable value

`concatMap()` always returns a new observable in a serialized way waiting all given observables completion before merging a new one

`finalize()` returns an Observable when the source terminates on complete or error.

Now the loading component `loading.component.html`

```html
<div *ngIf="loadingService.loading$ | async">
  <loading-component></loading-component>
</div>
```

Now `loading.component.ts`

```ts
export class LoadingComponent() {
  constructor(public loadingService: LoadingService)
}
```

Changing loading value in the component

In your component file you need to do the following config

```ts
export class HomeComponent implements OnInit {
  activeUsers$: Observable<User[]>;

  constructor(
    private loadigService: LoadinService,
    private usersService: UsersService
  );

  ngOnInit() {
    const users$ = this.usersService
      .loadAllUsers()
      .pipe(map((users) => users.sort(usersSortMethod)));

    const loadUser$ = this.loadingService.showLoaderUntilCompleted(users$);

    this.activeUsers$ = loadUser$.pipe(map((user) => user.active));
  }
}
```


Now you can call your loading component on your root level, ex: `app.component.html`

```html
<loading></loading>
<router-outlet></router-outlet>
```

Now you need to provide your LOadingService in the scope in need to, in this example we need to serve it at `app.component.ts`

```ts
  providers: [ LoadingService ]
```
