# SSRF (Server Side Request Forgery)

SSRF refers to an attack where an attacker is able to send crafted request from vulnerable web application.

Attacker use parameters used in web application to control request from vulnerable server.
e.g:. `http://websitename.com/?parameter=http://anotherwebsite.com` So server is making request behalf of attacker.

## Impact 

- Bypass ip whitelisting
- Bypass host based authentication
- Scan the internal network
- Abuse of vulnerable website to blacklist 

## 🔎 How to Find

Find any parameter that may have some kind of external interaction or they can interact to external domain.

Example: `http://example.com/profile/php?uri=http://externalwebsite.com `

## Possible Related parameter for SSRF

```
| dest | redirect | uri | path | continue | url | window | next | data | reference | site | html | val | validate 
| domain | callback | return | page | view | dir | show | file | document | folder | root | path | pg | style 
| pdf | template | php_path | doc | feed | host | port | to | out | navigation open result
```

## Exploitation

1. Read file from the server: `http://example.com/profile?file=file:///../etc/passwd` (here *../etc/passwd* is payload. replace with other payload)

2. Scan Internal network: ``
