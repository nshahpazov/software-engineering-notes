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