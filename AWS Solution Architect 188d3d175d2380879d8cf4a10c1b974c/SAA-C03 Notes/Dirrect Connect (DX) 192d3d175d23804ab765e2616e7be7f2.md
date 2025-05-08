# Dirrect Connect (DX)

- Provides a dedicated **private** connection from a remote network to VPC
- Dedicated connection must be setup between DC and AWS Direct Connect locations
- You need to setup a Virtual Private Gateway on your VPC
- Access public resources (s3) or private (ec2) on same connection
- Use cases:
    - Increase bandwidth throughput - working with large data sets - lower cost
    - More consistent network experience - app using real-time data feeds
    - Hybrid Env (on prem + cloud)
- Support both IPv4 and IPv6

# Direct Connect Diagram

AWS Direct Connect Location will have

- AWS Direct Connect Endpoint (AWS Cage)
- Customer or partner router (Customer or partner cage)

Remember this is physical network center. You want to request Direct Connect

- request to AWS Or AWS Direct Connect Partner (CMC, FPT, … )
- Partner and AWS Expert establish connection between your DC and AWS Direct Connect Location
- After that need some config on your DC

This is very complex process

Dirrect Connect Location will connect to your VPC using Virtual Private Gateway

# Direct Connect Gateway

- **If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway**

# Direct Connect - Connection Type

- **Dedicated Connection**: 1Gbps, 10Gbps, 100Gbps capacity
    - Physical ethernet port dedicated to a customer
    - Request made to AWS first, then complete by AWS Direct Connect Partner
- **Hosted Connection**: 50Mbps, 500 Mbps to 10 Gbps
    - Connection requests are made via AWS Direct Connect Partners
    - Capacity can **added or removed on demand**
    - 1,2,5,10 Gbps available at select AWS Direct Connect Partners
- Lead times are often longer than 1 month to establish a new connection

# Direct Connect - Encryption

- Data in transit is not encrypted but is private
- AWS Direct Connect + VPN provides an IPsec-encrypted private connection
- Good for an extra level of security, but slightly more complex to put in place

# Direct Connect - Resiliency

- High Resiliency for Critical Workloads
    - One connection at multiple locations
        - DCL1 - DC1
        - DCL2 - DC2
- Maximum Resilency for Critical Workloads
    - Seprate connections terminating on separate devices in more than one location
        - DCL1.1 - DC1.1 (on DCL1)
        - DCL1.2 - DC1.2 (on DCL1)
        - DCL2.1 - DC2.1 (on DCL2)
        - DCL2.2 - DC2.2 (on DCL2)

# Direct connect + Site to site VPN

In setup resiliency network on Direct connect

- **Solution 1: Direct Connect 1 (Primary) + Direct Connect 2 (Backup)**
    - high bandwidth/throughput network
    - very expensive
- **Solution 2: Direct Connect 1 (Primary) + Site to Site VPN (Backup) ⇒ Recommend**
    - backup network slower
    - much cheaper