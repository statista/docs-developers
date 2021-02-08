#VPC Setup

currently the VPC setup is done in three AWS regions, but can easily be applied to other regions. In this section the general setup is described.

<img src="../../images/vpc_simple.png" width=700>

## VPC-Level

__Naming__: _stat-vpc-{short-region-with-single-digit}-{two-digit-number}_, e.g. _stat-vpc-euc1-01_

Every VPC does have a IPv /16 address range assigned (65.535 IP addresses), we use the following format for this association:
_40.{country-predial-code}.0.0/16_, e.g. _40.1.0.0/16_ for stat-vpc-use1-01 and _40.49.0.0/16_ for stat-vpc-euc1-01. As there are a couple of regions in the USE region which would have the same pre-dial code we will simply count up in that case. 

We do assign Amazon IPv6 pools to every VPC as well.

## Subnet-Level

__Naming__: _{IPv4-CIDR-notation}-{short-availability-zone}-{public|private_, e.g. _40.1.1.0-use1a-public_

Every subnet does have a IPv /24 address range assigned (251 IP addresses) and every VPC does have two public and two private subnets. The first two subnets are public (e.g. _40.1.1.0/24_ and _40.1.2.0/24_) the next two subnets are private (e.g. _40.1.3.0/24_ and _40.1.4.0/24_).

We do assign Amazon IPv6 pools to every VPC as well, where the last two digits also refer to the 3rd digit from IPv4.