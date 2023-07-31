# unrepress

## A [Demo app](https://github.com/kataya1/unrepress-demo-app)

**The library exposes:**

route: a function you can use to add callback to a route and method combo

use: a function you can use to add middleware like cors

createServer: a function that creates the server

## simple example

```js
const { createServer, route, use } = require("unrepress");
const server = createServer();

route("/", "GET", (req, res) => {
  res.send("get /");
});

// Start server
server.listen(3000, () => {
  console.log("listening on port 3000");
});
```

## routes

### parameterized routes

```js
route("/users/:userId/posts/:postId", "GET", (req, res) => {
  const { userId, postId } = req.params;

  const user = users.find((u) => u.id === +userId);

  if (user) {
    const post = user.posts.find((p) => p.id === +postId);
    if (post) {
      res.json(post);
    } else {
      res.status(404).send("Post not found");
    }
  } else {
    res.status(404).send("User not found");
  }
});
```

### more ways for errors handling

```js
// error routes
route("/error1", "GET", (req, res) => {
  throw 418;
});

route("/error2", "GET", (req, res) => {
  res.error(418, "It's teatime !");
});
```

## middleware

```js
const cors = require("cors");

use(cors());
```

### custom middleware

```js
const middleware1 = (req, res, next) => {
  process.stdout.write("mw1👉");
  next();
};
const middleware2 = (req, res, next) => {
  process.stdout.write("mw2👉 ");
  next();
};
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url} - ${Date.now()}`);
  next();
};
use([middleware1, middleware2]);
use(logger);
```

### adding middleware for certain routes only

```js
const removePosts = (req, res, next) => {
  const modifiedUsers = users.map((user) => {
    const { posts, ...rest } = user;
    return rest;
  });

  // persist state/info from one middleware to the next
  res.locals.users = modifiedUsers;
  req.users = modifiedUsers;

  next();
};

route("/users", "GET", removePosts, (req, res) => {
  // res.json(req.users);
  res.json(res.locals.users);
});
```

## other usefull stuff the lib exposes

IncomingMessage: the Custom Incoming Message class the server uses. you can extend it and pass it to the options you can pass in the createServer function

ServerResponse: the Custom Server Response class

routes: a map of routes that maps from a string (the route you define using route) to an object example: { GET: RoutMethodInfo object, POST: {[], () => {}}}

routeCaller: a function that's called when a request event fires

middleware, an array of middleware functions

middleWareCaller, a function that calls the functions in the global middleware and local middleware array. takes req, res and a RouteMethodInfo object

paramRouteResolver: a function that converst a url with params like /users/12 to a url defined in the routes like /users/:user_id

RouteMethodInfo: a class that has two parameters, middleware: array of functions, and handler which is the route and method combo callback function

getRouteFromURL: a function that get's the route from the URL object. takes req.url

### advanced middleware example using the other stuff

```js
use((req, res, next) => {
  let route = getRouteFromURL(req.url);
  if (!routes.has(paramRouteResolver(getRouteFromURL(req.url)).paramRoute)) {
    res.error(400, `🤷‍♀️ ${route} not found`);
    return;
  }
  next();
});
```
