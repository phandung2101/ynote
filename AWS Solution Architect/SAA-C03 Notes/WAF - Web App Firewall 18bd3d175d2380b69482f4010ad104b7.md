# WAF - Web App Firewall

- protects your web applications from common web exploit ( layer 7 )
- **Layer 7 is HTTP (vs L4 is TCP/UDP)**
- Deploy on
    - ALB
    - API Gateway
    - CloudFront
    - AppSync GraphQL API
    - Cognito User Pool

# Web Appl Access  List

- Define Web ACL
    - IP Set
    - HTTP headers… to protect from **SQL injection** and **Cross-Site Scripting (XSS)**
    - size constraint, **geo-match (block countries)**
    - **rate-based rules - protect DDOS protection**
- Web ACL
    - regional except for CF
    - a rule group is **a reusable set of rules that you can add to a web ACL**

# WAF - Fixed IP while using WAF with a ELB

- WAF not support NLB (layer 4)
- We can use Global Accelerator for fixed IP and WAF on the ALB

![image.png](AWS%20Solution%20Architect/SAA-C03%20Notes/WAF%20-%20Web%20App%20Firewall%2018bd3d175d2380b69482f4010ad104b7/image.png)