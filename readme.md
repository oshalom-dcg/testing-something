## Schema

- `host` _&lt;string&gt;_ **required** - this is the host name of the api 
- `isHttps` _&lt;boolean&gt;_ *optional* - indicate whether the api domain is https, defaults to false
- `authentication` _&lt;object&gt;_ *optional*
    - `stradegy` _&lt;string&gt;_  **required** - oneOf ["FIREBASE"]
    - `placement` _&lt;object&gt;_ **required** where to place the JWT token or cookie
        - `type` _&lt;string&gt;_ **required** oneOf ["header", "payload", "query", "cookie"]
        - `key` _&lt;string&gt;_ **required** the name of the token or cookie to inject the value in
    - `name` _&lt;string&gt;_ **required** a name for this authentication to be referenced with other tests
    - `apiKey` _&lt;string&gt;_ **required** api key for the firebase app
    - `email` _&lt;string&gt;_ **required** email of the user to authenticate 
    - `password` _&lt;string&gt;_ **required** password of the user to authenticate 
- `tests` _&lt;Array&lt;object&gt;&gt;_ *optional* the tests in which will 
    - `name` _&lt;string&gt;_ **required** name of the test, can be anything but should be descriptive
    - `authentication` _&lt;string&gt;_ *optional* name of authentication to be used in the test
    - `path` _&lt;string&gt;_ **required** path of the api route
    - `method` _&lt;string&gt;_ **required** method of the api route to test

    


```js
module.exports = {
  host: "api.navarc.co",
  isHttps: false,
  authentication: {
    stradegy: "FIREBASE",
    placement: {
      type: "header",
      key: "idtoken",
    },
    name: "firebase-auth",
    apiKey: "AIzaSyCPRlgyIs3368E1RD4jpdWkavydRjhteps",
    email: "test@test.com",
    password: "password",
  },
  tests: [
    {
      name: "Score cards graph",
      authentication: "firebase-auth",
      path: "overview/scorecards/graph",
      method: "POST",
      metric: [
        "Total Sales",
        "Best Seller Rank (Median)",
        "Weighted Buy Box %",
        "Advertising Spend",
        "Total Sessions",
        "Products on Page 1",
        "Potential Sales Growth",
        "Advertising Saes",
      ],
      dateview: ["Day", "Week"],
    },
    {
      name: "overview scorecards",
      authentication: "firebase-auth",
      path: "overview/scorecards",
      method: "POST",
    },
  ],
};
```
