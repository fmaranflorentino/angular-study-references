## Error handling rxjs catch error

Let's say that we have the following Observable usage

```ts
const users$ = this.usersService.loadAllUsers();

this.activeUsers$ = users$.pipe(
  map((users) => courses.filter((user) => user.active))
);
```

Now we can easily threat the error in the Observable

```ts
const users$ = this.usersService.loadAllUsers();

this.activeUsers$ = users$.pipe(
  map((users) => courses.filter((user) => user.active)),
  catchError((err) => {
    // here you can track and log errors && whatever you want to do

    return throwError(err);
  })
);
```
