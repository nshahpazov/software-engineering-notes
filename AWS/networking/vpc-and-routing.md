# VPC and Routing

## VPC


With AWS VPC you can launch AWS resources in a logically isolated virtual network that you've defined. It's similar to a standard network in a data center, but with the ability to use AWS scalable infrastructure. VPC as a private neighborhood. VPC provides you with an address space ([CIDR blocks](https://aws.amazon.com/what-is/cidr/)). blocks). Individual IP addresses get assigned to interfaces/VMs inside subnets.

After you create a VPC, you can add subnets.

#### Subnets

A subnet is a range of IP addresses in your VPC. A subnet must reside in a single Availability Zone. After you add subnets, you can deploy AWS resources in your VPC.

#### IP Addressing

You can assign IP addresses, both IPv4 and IPv6 to your VPCs and subnets.  You can also allocate IP addresses to resources, such as EC2 instances, NAT gateways and Network load balancers.


CIDR blocks and maskings

| CIDR | Fixed Octets | Example Range | Meaning |
|------|---------------|----------------|----------|
| /8   | 10            | 10.0.0.0 → 10.255.255.255 | Only 10 is fixed — 16,777,216 IPs |
| /16  | 10.0          | 10.0.0.0 → 10.0.255.255   | First two fixed — 65,536 IPs |
| /24  | 10.0.0        | 10.0.0.0 → 10.0.0.255     | First three fixed — 256 IPs |
| /32  | 10.0.0.1      | single IP                | Fully fixed — 1 IP |


- 10.0.1.0 is the network address (not usable for hosts). Network addresses represent the subnet as a whole.
- 10.0.1.255 is the broadcast address (also not usable for hosts). Broadcast address is when you want to send packets to every IP in the in that subnet.

#### Routing and [Route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
Use [route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) to determine where network traffic from your subnet or gateway is directed.

A route table defines how outbound traffic from a subnet is directed.
Each subnet in a VPC is associated with one route table. Routes are evaluated top-down by destination CIDR:

VPC CIDR → local (always present) — enables internal communication within the VPC.

0.0.0.0/0 → igw-xxxx — sends Internet-bound traffic through an Internet Gateway (public subnet).

0.0.0.0/0 → nat-xxxx — sends Internet-bound traffic from private subnets through a NAT Gateway.

Other routes (e.g. 10.1.0.0/16 → pcx-xxxx, → tgw-xxxx, → vpce-xxxx) handle peering, transit gateways, or VPC endpoints (endpoints that are private connections between your VPC and supported AWS services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection).

[Route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) control egress paths.
Inbound traffic is governed separately by gateways and security groups.

#### Internet Gateway

A [gateway](https://docs.aws.amazon.com/vpc/latest/userguide/extend-intro.html) connects your VPC to another network. For example, use an internet gateway to connect your VPC to the internet. Use a VPC endpoint to connect to AWS services privately, without the use of an internet gateway or NAT device.


### VPC Endpoints
VPC Endpoints are endpoints that allow private connections between your VPC and supported AWS services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.