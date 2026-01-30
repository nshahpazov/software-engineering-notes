`# Networking Notes

## Reverse Proxy

A **reverse proxy** sits in front of backend services and handles incoming client traffic on their behalf. Common reverse proxy solutions:

- **AWS Application Load Balancer (ALB)**  
  Managed Layer 7 reverse proxy that routes HTTP/HTTPS traffic to EC2, ECS, EKS, and IP targets. Supports advanced routing, TLS termination, and WAF integration.

- **NGINX**  
  Popular open-source reverse proxy and web server. Deployed on EC2, ECS, or EKS. Known for performance, flexibility, and custom routing logic.

- **HAProxy**  
  High-performance open-source reverse proxy and load balancer for TCP and HTTP. Often used when you need granular control or extremely high throughput.

- **AWS CloudFront**  
  A global CDN that also acts as a reverse proxy. Requests are routed to edge locations and optionally cached to reduce latency and offload origin servers.

---

## AWS CloudFront

CloudFront is a globally distributed **CDN + reverse proxy** that delivers static and dynamic content with low latency.

### AWS Cloudfront Overview
AWS CloudFront is a content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency and high transfer speeds. It integrates with other AWS services to provide a seamless experience for developers and businesses looking to distribute content efficiently.

Note: You can reduce 503 (Gateway Timeout) errors by providing a failover origin (secondary origin) in CloudFront distribution settings. This way, if the primary origin becomes unavailable, CloudFront can automatically route requests to the secondary origin, ensuring continued availability of your content.


### AWS CloudFront Lambda@Edge
Lambda@edge is a feature of Amazon CloudFront that allows you to run AWS Lambda functions at AWS edge locations in response to CloudFront events. This enables you to customize the content that CloudFront delivers, execute code closer to your users, and improve performance by reducing latency. For example it can be used for: 
- validate a token/JWT at the edge,
- do redirects, header normalization, request routing,
- short-circuit obviously invalid/unauthorized requests,
- reduce repeated origin calls.
- Customize the content that the CloudFront web distribution delivers to your users using Lambda@Edge, which allows your AWS Lambda functions to execute the authentication process in AWS locations closer to the users.

This should improve performance and reduce load on origin servers by handling common tasks at the edge.
The functions run in response to CloudFront events, without provisioning or managing servers. You can use Lambda functions to change CloudFront requests and responses at the following points:
- After CloudFront receives a request from a viewer (viewer request)
- Before CloudFront forwards the request to the origin (origin request)
-  After CloudFront receives the response from the origin (origin response)
- Before CloudFront forwards the response to the viewer (viewer response)

### Key Features

1. **Global Edge Network**  
   Content is served from edge locations closest to users to minimize latency.

2. **Tight AWS Integration**  
   Works seamlessly with S3, ALB, EC2, API Gateway, Lambda, and more.

3. **Security**  
   - TLS/SSL support  
   - **Native AWS Shield DDoS protection**
   - Integration with AWS WAF for request filtering

4. **Customizable Delivery**  
   Cache behaviors, origin settings, and Lambda@Edge give fine-grained control over how requests/responses are handled.

5. **Metrics and Logging**  
   Real-time metrics and access logs to monitor traffic and performance.

6. **Cost-Effective**  
   Pay only for the data transfer and requests you use.

---

## CloudFront Caching Basics

A **cache key** is the set of request attr



## Application Load Balancer (ALB)
One of the main benefits that ALB provides is higher availability. It distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple Availability Zones (AZs). This ensures that if one instance or AZ goes down, the traffic can be routed to healthy instances in other AZs, minimizing downtime and improving fault tolerance. ALB is designed to handle varying levels of traffic and can automatically scale to meet demand, further enhancing availability. ALB is set for layer 7 (application layer) load balancing, which means it can make routing decisions based on the content of the request, such as URL path or host headers. This allows for more sophisticated routing strategies, such as directing traffic to different backend services based on the requested resource.


## Network Load Balancer (NLB)
NLB operates at the transport layer (Layer 4) and is designed to handle millions of requests per second while maintaining ultra-low latencies. It is optimized for sudden and volatile traffic patterns, making it suitable for applications that require extreme performance and scalability. NLB can handle TCP, UDP, and TLS traffic, making it versatile for various use cases. One of the key features of NLB is its ability to preserve the source IP address of the client, which is important for applications that need to see the original client IP for logging or security purposes. NLB also supports static IP addresses and Elastic IP addresses, providing a fixed entry point for applications. Additionally, NLB is capable of handling sudden spikes in traffic without the need for pre-warming, making it a robust choice for high-throughput applications. It can be used for streaming applications, gaming, IoT, and other scenarios that demand high performance and low latency.

Note: NLB does not provide advanced routing features like ALB, as it operates at a lower layer of the OSI model. It doesn't have weighted target groups or host-based routing capabilities.