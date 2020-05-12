## Avoid multiple HTTP request with shareReplay

Let's use the following service as an example

`shareReplay()` locates the request value in memory so, when the observable gets subscribed again it's just proxy the value

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
      .pipe(
        map((response) => res["payload"], shareReplay()
      );
  }
}
```
