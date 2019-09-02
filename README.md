# Lagom Pac4j Demo: How authenticate/authorize by JWT

This recipe demonstrates, how you can use [JWT](https://en.wikipedia.org/wiki/JSON_Web_Token) 
for protection your service endpoints. It uses the [PAC4J](https://www.pac4j.org/) library and 
its [lagom-pac4j module](https://github.com/pac4j/lagom-pac4j) for Lagom integration.

## About service

Service has two methods. Both returns a simple string with the profile id.

* `/authenticate`. It's a public method accessed for all users. 
* `/authorize`. It's a protected method accessed only users with role 'manager'.

## Testing the recipe

_Note: All JWT from tests you can to analyze on the site https://jwt.io/. JWKs and JWTs for tests generated by `JWTTestDataGenerator`._

#### unit tests

You can test this recipe using the provided tests:

```bash
./mvnw test
```

#### manual tests

You can also test this recipe manually using 2 separate terminals.

On one terminal start the service:

```bash
./mvnw lagom:runAll
```

On a separate terminal, use `curl` to send:

* anonymous request
```bash
$ curl http://localhost:9000/authenticate
anonymous
```

* request from Alice
```bash
$ curl -H "Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJBbGljZSIsInJvbGVzIjpbIm1hbmFnZXIiXSwiaXNzIjoiaHR0cHM6XC9cL3BhYzRqLm9yZyIsImlhdCI6MTU0NzIzNzI4MywianRpIjoiYTA4NzM5M2UtMDQxMS00ZjNlLThmNjgtN2Y5NjAwNTM4ZmUwIn0.YuhvguJ83BacWIY3B_10xPQLIDvhrfmWFLpWiPRMo2g-EhRKxktux0XWHVPlLCzcKlZCQRGOGrQr7mwR0QpofJYEuFppITneDxNrIO3Mmy5kqvBusv9TnOydMB5GRjYXZxBnGaioRt2dG-6eQRJdJyRu87jxn6o6RpOGg3KpvCPqs8RJWBYRW6C_5NibkU99TUhdpLyfEX8dZ2Xj74RWZUN9kFA3JysY83OWJBh-gfuDpcfhJmmQYsK5z5UL5oxmoBQBYWfgaRA3qKqwwEx4h0pXn9KwwAo5D3qA_GkJlebOZejjN6DhDxyYcklo_a9ghBAso2a3msYYtDlitabpOw" http://localhost:9000/authenticate
Alice
```

* request from Alice to protected path (Alice has role _manager_) 
```bash
$ curl -H "Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJBbGljZSIsInJvbGVzIjpbIm1hbmFnZXIiXSwiaXNzIjoiaHR0cHM6XC9cL3BhYzRqLm9yZyIsImlhdCI6MTU0NzIzNzI4MywianRpIjoiYTA4NzM5M2UtMDQxMS00ZjNlLThmNjgtN2Y5NjAwNTM4ZmUwIn0.YuhvguJ83BacWIY3B_10xPQLIDvhrfmWFLpWiPRMo2g-EhRKxktux0XWHVPlLCzcKlZCQRGOGrQr7mwR0QpofJYEuFppITneDxNrIO3Mmy5kqvBusv9TnOydMB5GRjYXZxBnGaioRt2dG-6eQRJdJyRu87jxn6o6RpOGg3KpvCPqs8RJWBYRW6C_5NibkU99TUhdpLyfEX8dZ2Xj74RWZUN9kFA3JysY83OWJBh-gfuDpcfhJmmQYsK5z5UL5oxmoBQBYWfgaRA3qKqwwEx4h0pXn9KwwAo5D3qA_GkJlebOZejjN6DhDxyYcklo_a9ghBAso2a3msYYtDlitabpOw" http://localhost:9000/authorize
Alice
```

* request from Bob to protected path (Bob has role _developer_)
```bash
$ curl -H "Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJCb2IiLCJyb2xlcyI6WyJkZXZlbG9wZXIiXSwiaXNzIjoiaHR0cHM6XC9cL3BhYzRqLm9yZyIsImlhdCI6MTU0NzIzNzI4MywianRpIjoiNGJjNGNmNjEtYzVkYy00ODM5LWFiN2YtN2QyY2MxNDEwNTJiIn0.THmUlOWU6Mw1D5wt5FoBUClqWCI3AQ9om8l1YZ4U4m19QTSugZcWQFZUnA1AAYEcSJzHUvGezlYouFRF7CFds7tKE4Xyt7rglfjMnVqa1s6rAMG8zSEjNFsQ2k-nzNOy9V5YAEtLndHXOz6bek6cnIU0h8eUrYqm8R1DpFRu_pVewl008UF-iOpQJoYzg0wsSPp4ZPePrXOM5SCCDiUQ2kUBEHxRw6PVk34aXgSUHJC8aaK5Tt5GaOyI6fjIAvstZWFnXMpcUKylIUwdwKqybkoJYCxaK5_Q2otx6ImVfrLQSnjdL4QmhJvfcT19ZaEKS1Drmv8u-d3J6e0BdqodJg" http://localhost:9000/authorize
{"name":"Forbidden","detail":"Authorization failed"}
```
