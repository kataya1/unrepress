## 1.0.1

- clean up some files

## 1.0.0

- published

## 0.2.3

- Exported more things.
- used external middleware libraries like CORS (works now)
- bug: cors work but you get both cors error and 404 for undefined routes, Fix: need to change the caller function so that middleware runs regardless if there is a route or a handler
- didn't need to modify the middlewareCaller function for the fix
- fixed some bugs that made the lib behave irruglargly if the user tried to make custom error middleware
- renamed some stuff caller -> routeCaller, route (returned from paramRouteResolver) -> paramRoute.
- changed the catch block to call middlewareCaller instead of the try block

## 0.2.2

- modified "use()" so that it can accept an array of functions
- modified customServRspnse to have "locals" object (persist data/state between middleware using it)

## 0.2.1

- Middle ware: add middleware function with "use" and can add multiple middleware function in "route" handler -> handlerObj {middleware ,handler}. why use "next()" don't know, just making it like express

## 0.2.0

- renaming "assign" to "route", removing the builtin server app, added res.status(xxx).send(m)

## 0.1.6

- module, export assign, (how do you make the user create a server using the custom classes? export a custom createServer?, what if he wanted to pass his own options, does express expose it's own custom incomingMessage class, serverResponse class?)
- "if (require.main === module)" so that the builtin server app wont run when an external code import the module

## 0.1.5

- extending http.ServerResponse to have res.json() and res.send()

## 0.1.4

- ERROR HANDLING (after deleperation we made res.error(status, message?))

## 0.1.3

- users/:id URL Parameters ( TrieNode, buildParamRouteTree, paramRouteResolver)

## 0.1.2

- caller function: function that calls routes
- bugs: makerout function , trailing slashes
- Handle errors centrally, like 404s. Handle 404 errors for undefined routes at the end.

## 0.1.1

- Verify method

## 0.0.0

- I'll start with a basic idea.
- I'll have a hash map. that have the routes as a key.
- The value is an object with the functions corresponding to the methods.
- the functions of course take the req and res inputs.
