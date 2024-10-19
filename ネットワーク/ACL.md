A Network Access Control List (ACL) is a security layer for your network that controls traffic in and out of one or more subnets. Here's an overview of network ACLs:

1. Definition:
    - A network ACL is an optional layer of security that acts as a firewall for controlling traffic in and out of subnets in a Virtual Private Cloud (VPC).
2. Key characteristics:
    - Stateless: They evaluate inbound and outbound traffic separately.
    - Subnet level: Applied at the subnet boundary, not to individual instances.
    - Rule-based: Uses numbered rules to allow or deny traffic.
3. Rule components:
    - Rule number
    - Type of traffic (e.g., SSH, HTTP)
    - Protocol (TCP, UDP, ICMP, etc.)
    - Port range
    - Source or destination (IP address range)
    - Allow or Deny action
4. Processing order:
    - Rules are processed in order, from lowest to highest number.
    - The first matching rule is applied, regardless of subsequent rules.
5. Default behavior:
    - Most cloud providers include a default network ACL that allows all inbound and outbound traffic.
6. Comparison to security groups:
    - While security groups operate at the instance level, network ACLs work at the subnet level.
    - Network ACLs provide an additional layer of security beyond security groups.
7. Use cases:
    - Blocking specific IP ranges or ports at the subnet level.
    - Implementing network-wide policies that apply to all instances in a subnet.
8. Limitations:
    - Can't be used to block specific instance-to-instance traffic within the same subnet.
    - More complex to manage than security groups due to their stateless nature.