## Schema

- `host` *&lt;string&gt;* (required)

```js
module.exports = {
  host: "api.navarc.co",
  isHttps: false,
  authenication: {
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
      authenication: "firebase-auth",
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
      authenication: "firebase-auth",
      path: "overview/scorecards",
      method: "POST",
    },
  ],
};
```
