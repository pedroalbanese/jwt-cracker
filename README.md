# JWT-Cracker
[![ISC License](http://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/pedroalbanese/jwt-cracker/blob/master/LICENSE.md) 
[![GoDoc](https://godoc.org/github.com/pedroalbanese/jwt-cracker?status.png)](http://godoc.org/github.com/pedroalbanese/jwt-cracker)
[![GitHub downloads](https://img.shields.io/github/downloads/pedroalbanese/jwt-cracker/total.svg?logo=github&logoColor=white)](https://github.com/pedroalbanese/jwt-cracker/releases)
[![Go Report Card](https://goreportcard.com/badge/github.com/pedroalbanese/jwt-cracker)](https://goreportcard.com/report/github.com/pedroalbanese/jwt-cracker)
[![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/pedroalbanese/jwt-cracker)](https://golang.org)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/pedroalbanese/jwt-cracker)](https://github.com/pedroalbanese/jwt-cracker/releases)

HS256/384/512 JWT token brute force cracker.

This is realistically only effective to crack JWT with weak secrets. It also only currently works with HMAC-SHA2 signatures.

It should be slightly faster than it's inspiration, as it uses a new goroutine for each generated and compared hash. Could be made faster if it was generating secrets in more than one goroutine.

## Usage
```
Usage of go-jwt-cracker:
  -alphabet string
       The alphabet to use for the brute force (default "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789")
  -maxlen int
       The max length of the string generated during the brute force (default 12)
  -prefix string
       A string that is always prefixed to the secret
  -suffix string
       A string that is always suffixed to the secret
  -token string
        The full HS256 jwt token to crack
```

## Example
Cracking a token generated with [jwt.io](https://jwt.io):

```bash
jwt-cracker -token "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.XbPfbIHMI6arZ3Y922BhjWgQzWXcXNrz0ogtVhfEd2o" -alphabet "abcdefghijklmnopqrstuwxyz" -maxlen 6
```

### Output

```
Parsed JWT:
- Algorithm: HS256
- Type: JWT
- Payload: {"sub":"1234567890","name":"John Doe","iat":1516239022}
- Signature (hex): 5db3df6c81cc23a6ab67763ddb60618d6810cd65dc5cdaf3d2882d5617c4776a

There are 254313150 combinations to attempt
Cracking JWT secret...
Attempts: 100000
Attempts: 200000
Attempts: 300000
...
Attempts: 184500000
Attempts: 184600000
Attempts: 184700000
Found secret in 184776821 attempts: secret
```

### Time spent
- Intel Core i7-4790k @ 4.38GHz - around 4.5 minutes
- Intel Xeon E3-1270 V2 @ 3.50GHz - around 15 minutes
