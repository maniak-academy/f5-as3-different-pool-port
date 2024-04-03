# f5-as3-different-pool-port

Here is an example of a AS3 template for F5 with pool members having different ports.

```
{
   "class": "AS3",
   "action": "deploy",
   "persist": true,
   "declaration": {
     "class": "ADC",
     "schemaVersion": "3.0.0",
     "id": "123abc",
     "label": "HTTPS Example",
     "remark": "HTTPS with round-robin pool",

     "Tenant_prod": {
       "class": "Tenant",

       "prod_https": {
         "class": "Application",

         "prod_https_vs": {
           "class": "Service_HTTPS",
           "virtualAddresses": [
             "172.16.40.12"
           ],
           "pool": "prod_https_pool",
           "serverTLS": "prod_https_tls"
         },

         "prod_https_pool": {
           "class": "Pool",
           "loadBalancingMode": "round-robin",
           "monitors": [
            "http"
           ],

           "members": [{
             "servicePort": 80,
             "shareNodes": true,
             "serverAddresses": [
               "172.16.1.100",
               "172.16.1.101"
             ]
           },
           {
             "servicePort": 8080,
             "shareNodes": true,
             "serverAddresses": [
               "172.16.1.102",
               "172.16.1.103"
             ]
           }
           ]
         },

         "prod_https_tls": {
           "class": "TLS_Server",
           "certificates": [{
             "certificate": "prod_https_cert"
           }]
         },

         "prod_https_cert": {
           "class": "Certificate",
           "remark": "In practice we recommend using a passphrase",
 "certificate": "-----BEGIN CERTIFICATE-----\nMIIDSDCCAjCgAwIBAgIEFPdzHjANBgkqhkiG9w0BAQsFADBmMQswCQYDVQQGEwJVUzETMBEGA1UECBMKV2FzaGluZ3RvbjEQMA4GA1UEBxMHU2VhdHRsZTESMBAGA1UEAxMJQWNtZSBDb3JwMRwwGgYJKoZIhvcNAQkBFg10ZXN0QGFjbWUuY29tMB4XDTIxMDIyMzE1MjYyMloXDTIyMDIyMzE1MjYyMlowZjELMAkGA1UEBhMCVVMxEzARBgNVBAgTCldhc2hpbmd0b24xEDAOBgNVBAcTB1NlYXR0bGUxEjAQBgNVBAMTCUFjbWUgQ29ycDEcMBoGCSqGSIb3DQEJARYNdGVzdEBhY21lLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMlj0IAPjyzSJzJZXzPPKnWWu6ErkfUhujBk1nQdp++K8YrvocLY5rbY/DZ9ipEz1TxIyZAGl9aBPoLGU4r2XNVMJZA5Zo/QMedEZFal14eOHn3JMMvCDjGrFaWR+cZ2TR9D9Jdxytq9QKUS/JdCKYz/Qxbx2Q4o0nZDqmPAipw//E24y4lLya/Qrwb537QoCGnxgMoUVNi9ruYrnGtS4uPth3+jYmbbivb8G1B8x93Peeu1QHBlOaXr6DndSwKcULIUt2RxcyYcjzeVQ5yrcrVHwvF8eb03Llm10zX0UI68vdjQuMpeUZ7K0RlmO59v0u6XdeoK1s82SJ8ufscqIrcCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAN78Mxd1ZE9nUNefCxmqdJYbQUzO6baHgqWxNLRuIu/EtJsjBAuuMmpWOd+pWBQS42aDOrUc33zpcNJcquBvtKt6QaKnSzJwCyfk88ACNrb7yFyeKB3YhVALfLkJMal032pvV8U0n4FBlqRTUDrSY2MHaJ/Uar7iJ7t3RBoZ9LbTyikW188hW6h9s238oLOW89FIJluov18uyLJaj8sBP5tInZmnO3EEywzNop0vpqMe0XmTo9Dyq9SFRdcDnptSdoNLLWTXmpXacj/u/f9r7zQqneFbj2b0KqetYLb7Xs5BVi0DfC81FYOEwqiq+kYvEkBubNCP1C8fXzB/65kFXtg==\n-----END CERTIFICATE-----",
 "privateKey": "-----BEGIN PRIVATE KEY-----\nMIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQDJY9CAD48s0icyWV8zzyp1lruhK5H1IbowZNZ0HafvivGK76HC2Oa22Pw2fYqRM9U8SMmQBpfWgT6CxlOK9lzVTCWQOWaP0DHnRGRWpdeHjh59yTDLwg4xqxWlkfnGdk0fQ/SXccravUClEvyXQimM/0MW8dkOKNJ2Q6pjwIqcP/xNuMuJS8mv0K8G+d+0KAhp8YDKFFTYva7mK5xrUuLj7Yd/o2Jm24r2/BtQfMfdz3nrtUBwZTml6+g53UsCnFCyFLdkcXMmHI83lUOcq3K1R8LxfHm9Ny5ZtdM19FCOvL3Y0LjKXlGeytEZZjufb9Lul3XqCtbPNkifLn7HKiK3AgMBAAECggEAHrvaWmjFd1gc/jyQYFY5yxc1TCvbivbaNL920OKjudVQ9lyKqbMzRm1H1EMFbhJkdN5A0HeJHYW81fVRU5A0a6LCysdPxRvHOd2AmI6XnUrNkXGuPjI/u0m6NHnaDfUI4QAcaC5IAGjIYEjM/oJs1+UuxmYjM1t8furlqnJ8VMrTtfumTSzEoq3OSwK6JlxAtRwBkrYr+pZCI82Ao/ouZcktyO+LjLMCYd/7HpskA0G3Pp/SIjhfbNHsTm4Kvzar0XdxBVMu4M0a6xXvX+I/L8UFa7EuDumwQVdttMnG33+xbCX3yIrndxVk0rOCryYqeItOxSG+aZJ6ZN+r0crgvQKBgQDlolxVh3NQCqa9XS+TCXSGfcZENolAIEu+n6CkZKMqQrmz7KPrblHgMbc54x0jH/GhtkY4wMjV1stqqHWTBV9UqVcb+OXPt8Zwa0JvXEM3b0i9GTp2hQfKGbWTEz7PIUZbZPGrIijHrc4wX2rgq7K5GeUfteh7bZij0cQ5ubFXBQKBgQDgg0SzUIN87D7yRPqvVnd7l0yii740UztWDp/YDpml0T6WfG0rlXqnIEebUS+BxGNDt3ksG7dTfJIE34C/SlYBAlJ5hiJ4rErW7pm+ibLa96qnLAGEQpiwnYmV49Uc2mxxRuBFWqKal4tuY6MxNqaf2NLCT4JKTPMffRR0Iz/HiwKBgFN659hMCpaxmJZE1zO7/zmZZceMj+7ZDtA41byNvWdypHINeDXxgCBh0ntf3krTpRMl4XdmVlyu3npizYNqM5LikQFhRaJy69gYlilHwEPZ1/auwjst93v4RrM2DuJb9WjqVJTjMTIONGQPfBo7MRjrmgkiJ2cfm5sKeiyGHjtFAoGAOvwB1qJ2iSGAQCJDQkGTTpMnfST9qb2cPzXEZP0g/OGGcf7qp6K0AKiIZ5PiyVMRST8wxJfbiEGYE1Os/ZTIF6fGh0roT4/kcadqGRcQOFsNKLJ1C4x7lRsuhITA/r2b8/7M+SugwMDDzxK6UzmqeSB77rT45BBnZ4RzFTgVj5UCgYBasm6nh2v8PzE8aXzYM6YWsM5R0l3Xr2YvycLc6HfI5Sen+yqIyE9qDNFFk8qa3RZVKl83ZTk7wtAL8nMupmdVodNrx4pUgFW4U4WPPSvVXRZRTZdLHwGKw5Fa3u48qFxdYqpZmnBMB2RHEKcRB8T1+RkcByghNnCBwJMJSRHIWA==\n-----END PRIVATE KEY-----"
         }
       }
     }
   }
}
```
