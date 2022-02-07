### Observer design pattern (in rxjs)

In CS2103/T, I learnt about the Observer design pattern but I did not really see it being used often until I had to work with rxjs in Angular.

Observables are used in Angular to provide a form of lazy transfer of data to the observer in both a synchronous and asynchronous manner, in a beautiful functional programming style. For example, if we want to get API data of the authenticated user from github, we can call

```typescript
const observable = from(octokit.users.getAuthenticated()).pipe(
    map((response) => response['data']),
    catchError((err) => throwError('Failed to fetch authenticated user.'))
);
```

`from(...)` will convert the `Promise` object `octokit.users.getAuthenticated()` into an observable. We can then change the values returned by this promise by using `pipe(...)`. In the example, the `map(...)` function in the pipe will only retrieve the `'data'` values of the response from the API call. Afterwards, we can subscribe to the observable somewhere else by calling `subscribe(...)`. In the example below, we will subscribe to the observable and print the data to the console.

```typescript
observable.subscribe((data) => console.log(data));
```

rxjs provides a wide range of transformation operators (such as map) that will make programming with lazy data (like asynchronous API calls) easier. For more information, visit [https://rxjs.dev/guide/overview](https://rxjs.dev/guide/overview).
