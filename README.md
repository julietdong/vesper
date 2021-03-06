# vesper
SHAKEN based signing and validation server

## APIs

### POST /stir/v1/signing

#### HTTP Response

##### Success	

###### 200 OK

Example
```
{
  "signingResponse": {
    "identity": "eyJhbGciOiJFUzI1NiIsInR5cCI6InBhc3Nwb3J0IiwicHB0Ijoic2hha2VuIiwieDV1IjoiaHR0cHM6Ly9jZXJ0LWF1dGgucG9jLnN5cy5jb21jYXN0Lm5ldC9leGFtcGxlLmNlciJ9.eyJhdHRlc3QiOiJBIiwiZGVzdCI6eyJ0biI6WyJmZHN2Z2FzIl19LCJpYXQiOjMyMzEyMzQxLCJvcmlnIjp7InRuIjoiZHdkdyJ9LCJvcmlnaWQiOiJkc2RzZCJ9.xhVsurlxeS8cidMQxnQKb9ZLLQwgzYjMhZ_crGXpT115RrcYYRpCT1qU421ttHSAlwX_LsR8HXf_F6WSyBpuJw;info=<https://cert-auth.poc.sys.comcast.net/example.cer>;alg=ES256"
  }
}
```

##### Unsuccessful

###### 400

Example
```
{
  "signingResponse": {
    "reasonCode": "VESPER-0002",
    "reasonString": "Unable to parse request body"
  }
}
```

| reasonCode | reasonString |
| ----- | ----- |
| VESPER-0001 | empty request body |
| VESPER-0002 | Unable to parse request body |
| VESPER-0003 | one or more of the require fields missing in request payload |
| VESPER-0004 | request payload has more than expected fields |
| VESPER-0005 | attest field in request payload is an empty string |
| VESPER-0006 | attest field in request payload is not as per SHAKEN spec |
| VESPER-0007 | attest field in request payload MUST be a string |
| VESPER-0008 | iat value in request payload is 0 |
| VESPER-0009 | iat field in request payload MUST be a number |
| VESPER-0010 | origid field in request payload is an empty string |
| VESPER-0011 | origid field in request payload MUST be a string |
| VESPER-0012 | orig in request payload is an empty object |
| VESPER-0013 | orig in request payload should contain only one field |
| VESPER-0014 | orig in request payload does not contain field \"tn\" |
| VESPER-0015 | orig tn in request payload is not of type string |
| VESPER-0016 | orig tn in request payload is an empty string |
| VESPER-0017 | orig field in request payload MUST be a JSON object |
| VESPER-0018 | dest in request payload is an empty object |
| VESPER-0019 | dest in request payload should contain only one field |
| VESPER-0020 | dest in request payload does not contain field \"tn\" |
| VESPER-0021 | dest tn in request payload is an empty array |
| VESPER-0022 | one or more dest tns in request payload is not a string |
| VESPER-0023 | one or more dest tns in request payload is an empty string |
| VESPER-0024 | dest tn in request payload is not an array |
| VESPER-0025 | dest field in request payload MUST be a JSON object |

###### 500

Example
```
{
  "signingResponse": {
    "reasonCode": "VESPER-0152",
    "reasonString": "error in signing request"
  }
}
```

| reasonCode | reasonString |
| ----- | ----- |
| VESPER-0050 | error in converting header to byte array |
| VESPER-0051 | error in converting claims to byte array |
| VESPER-0052 | error in signing request |


### POST /stir/v1/verification

#### HTTP Response

##### Success

###### 200

Example
```
{
  "verificationResponse": {
    "dest": {
      "tn": [
        "1215345567"
      ]
    },
    "iat": 1504282247,
    "jwt": {
      "claims": {
        "attest": "A",
        "dest": {
          "tn": [
            "1215345567"
          ]
        },
        "iat": 1504282247,
        "orig": {
          "tn": "12154567894"
        },
        "origid": "1db966a6-8f30-11e7-bc77-fa163e70349d"
      },
      "header": {
        "alg": "ES256",
        "ppt": "shaken",
        "typ": "passport",
        "x5u": "https://cert-auth.poc.sys.comcast.net/example.cer"
      }
    },
    "orig": {
      "tn": [
        "12154567894"
      ]
    }
  }
}
```

##### Unsuccessful

###### 400

Example
```
{
  "verificationResponse": {
    "reasonCode": "VESPER-0100",
    "reasonString": "Unable to parse request body"
  }
}
```

| reasonCode | reasonString |
| ----- | ----- |
| VESPER-0100 | empty request body |
| VESPER-0102 | Unable to parse request body |
| VESPER-0103 | one or more of the require fields missing in request payload |
| VESPER-0104 | request payload has more than expected fields |
| VESPER-0105 | iat value in request payload is 0 |
| VESPER-0106 | iat field in request payload MUST be a number |
| VESPER-0107 | identity field in request payload is an empty string |
| VESPER-0108 | attest field in request payload MUST be a string |
| VESPER-0109 | orig in request payload is an empty object |
| VESPER-0110 | orig in request payload should contain only one field |
| VESPER-0111 | orig in request payload does not contain field \"tn\" |
| VESPER-0112 | orig tn in request payload is an empty array |
| VESPER-0113 | one or more orig tns in request payload is not a string |
| VESPER-0114 | one or more orig tns in request payload is an empty string |
| VESPER-0115 | orig tn in request payload is not an array |
| VESPER-0116 | orig field in request payload MUST be a JSON object |
| VESPER-0117 | dest in request payload is an empty object |
| VESPER-0118 | dest in request payload should contain only one field |
| VESPER-0119 | dest in request payload does not contain field \"tn\" |
| VESPER-0120 | dest tn in request payload is an empty array |
| VESPER-0121 | one or more dest tns in request payload is not a string |
| VESPER-0122 | one or more dest tns in request payload is an empty string |
| VESPER-0123 | dest tn in request payload is not an array |
| VESPER-0124 | dest field in request payload MUST be a JSON object |
| VESPER-0125 | Invalid JWT format in identity header in request payload |
| VESPER-0126 | decoded header does not have the expected number of fields (4) |
| VESPER-0127 | one or more of the required fields missing in JWT header |
| VESPER-0128 | alg field value in JWT header is not \"ES256\" |
| VESPER-0129 | alg field value in JWT header is not a string |
| VESPER-0130 | ppt field value in JWT header is not \"shaken\" |
| VESPER-0131 | ppt field value in JWT header is not a string |
| VESPER-0132 | typ field value in JWT header is not \"passport\" |
| VESPER-0133 | typ field value in JWT header is not a string |
| VESPER-0134 | x5u field value in JWT header is not a string |

###### 500

Example
```
{
  "verificationResponse": {
    "reasonCode": "VESPER-0150",
    "reasonString": "unable to base64 url decode header part of JWT"
  }
}
```

| reasonCode | reasonString |
| ----- | ----- |
| VESPER-0150 | unable to base64 url decode header part of JWT |
| VESPER-0151 | unable to unmarshal decoded JWT header |
| VESPER-0152 | unable to base64 url decode claims part of JWT |
| VESPER-0153 | unable to unmarshal decoded JWT claims |
| VESPER-0154 | STRING WILL DEPEND ON ERROR DETECTED |
