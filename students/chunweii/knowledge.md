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

### Logging

Though it might seem to be redundant at first, log messages are helpful to developers when users report a bug that cannot be resolved easily. In CATcher, logging is used at various levels, from logging error messages to logging network HTTP requests to the github server. The octokit API library that is used to perform API requests to the Github server allows developers to customize the various logging levels, so that there is no need to manually hard-code the logging messages for every method that has a Github API call. By separating the various logging levels, developers can easily filter through the less important messages if there is a critical error. There are usually 4 levels of logging for the console (other than the general `console.log`), shown below in increasing severity:

1. **debug**: Provides useful information to the developer in development mode. Usually debug messages are not very useful in production, especially for CATcher, since they take up a lot of space in the log. An example of a debug message can be the HTTP request details of an API call to Github.
2. **info**: Provides information about a less important event occuring or a state change. Such information is not very important to the developers, but in certain situations, the developer may find such messages helpful.
3. **warn**: Provides an important warning to the developer if there is an unexpected situation or bad code, but the application can continue to work. Developers should take note of such warnings and address these warnings if possible.
4. **error**: Provides an important error message to the developer if one or more components of the application fails and stop functioning properly. This usually constitutes as a bug that developers should work on promptly.
