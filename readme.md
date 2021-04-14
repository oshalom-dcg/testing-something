# Testiny (A declarative way to test your REST APIs)

![testiny logo](testiny_logo-200x200.png)


## JSON Testing Schema

- `host` _&lt;string&gt;_ **required** - this is the host name of the api 
- `isHttps` _&lt;boolean&gt;_ *optional* - indicate whether the api domain is https, defaults to false
- `authentication` _&lt;object&gt;_ *optional*
    - `strategy` _&lt;string&gt;_  **required** - oneOf ["FIREBASE"]
    - `placement` _&lt;object&gt;_ **required** where to place the JWT token or cookie
        - `type` _&lt;string&gt;_ **required** oneOf ["header", "payload", "query", "cookie"]
        - `key` _&lt;string&gt;_ **required** the name of the token or cookie to inject the value in
    - `name` _&lt;string&gt;_ **required** a name for this authentication to be referenced with other tests
    - `apiKey` _&lt;string&gt;_ **required** api key for the firebase app
    - `email` _&lt;string&gt;_ **required** email of the user to authenticate 
    - `password` _&lt;string&gt;_ **required** password of the user to authenticate 
- `tests` _Array&lt;object&gt;_ *optional* the tests in which will 
    - `name` _&lt;string&gt;_ **required** name of the test, can be anything but should be descriptive
    - `authenticated` _&lt;boolean&gt;_ *optional* whether or not this request should be authenticated default false
    - `path` _&lt;string&gt;_ **required** path of the api route
    - `method` _&lt;string&gt;_ **required** method of the api route to test
    - `payloads` _&lt;object&gt;_ *optional* method of the api route to test
    


## Example file

```js
module.exports = {
  host: "api.todo.com",
  isHttps: true,
  authentication: {
    strategy: "FIREBASE",
    placement: {
      type: "header",
      key: "Token",
    },
    apiKey: "KSzanhFYRlgyIs3368E1RD4jpdWkavydRjhtfee",
    email: "mrcool@gmail.com",
    password: "myPassword123",
  },
  tests: [
    {
      name: "get all todos",
      authenticated: false,
      path: "todos/all",
      method: "GET"
    },
    {
      name: "add todos",
      authenticated: true,
      path: "todos/add",
      method: "POST",
      payload: {
        todo: ["get milk", "drive car", "shower"]
      }
    },
  ],
};
```

The above json file will be parsed and converted to 4 tests dynamically, it will have the following generated and runned:

```http
GET https://api.todo.com/todos/all
token: <id token value here>
```

```
POST https://api.todo.com/todos/add HTTP/1.1
Content-Type: application/json
Token: <id token value here>

{
    "todo": "get milk"
}
```

```
POST https://api.todo.com/todos/add HTTP/1.1
Content-Type: application/json
Token: <id token value here>

{
    "todo": "drive car"
}
```

```
POST https://api.todo.com/todos/add HTTP/1.1
Content-Type: application/json
Token: <id token value here>

{
    "todo": "shower"
}
```
