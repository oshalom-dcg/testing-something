# Testiny 

> A declartive way to test RESTful API's with a single JS config.

<img src="testiny_logo-200x200.png" align="right"
     alt="Size Limit logo by Anton Lovchikov">

Testiny is a API testing tool for any language. It checks a single JS config file and parses to generate tests. Every generate will run and produce either a sucess or an error. 

![demo](https://user-images.githubusercontent.com/70663071/114795232-b6d21600-9d53-11eb-862f-0e168deceb4e.mov)

* **ES modules** and **tree-shaking** support.
* Add Size Limit to **Travis CI**, **Circle CI**, **GitHub Actions**
  or another CI system to know if a pull request adds a massive dependency.
* **Modular** to fit different use cases: big JS applications
  that use their own bundler or small npm libraries with many files.
* Can calculate **the time** it would take a browser
  to download and **execute** your JS. Time is a much more accurate
  and understandable metric compared to the size in bytes.
* Calculations include **all dependencies and polyfills**
  used in your JS.





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
