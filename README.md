# Fortresser

Fortresser is a security group and access control list design tool. By defining your infrastructure components and ports used as code, you can generate a design suitable for your security needs on many popular service providers.

It does _not_ actually create objects on your service provider accounts. It only provides a design for you to implement by other means.

## Project Definition

A project definition is a directory of files that describe your networks, subnets, and services. You may specify network rules on any object.

### Network

A network is a container for other component types.

```hcl
network "vnet" {

  description = "This represents a virtual network."

  ingress {
    access = "deny" # alternative and default value is "allow".
    from = ["40.41.42.43"]
    ports = ["all"] # "all" keyword indicates ports 1 - 65535.
    protocols = ["any"] # indicates tcp, udp, and icmp
  }
}
```

### Subnet

Subnets are a smaller division inside of networks for grouping like services.

```hcl
subnet "public" {

  description = "This represents my public subnet."

  network = "vnet"

  egress {
    access = "deny"
    to = ["all"]
    ports = ["all"]
    protocols = ["any"]
  }
}
```

### Service

Services are the components that need network access in some way.

```hcl
service "thing" {

  description = "This is a service that needs some kind of network access."

  subnet = "public"

  ingress {
    from = ["all"] # "all" keyword is the same as 0.0.0.0/0.
    ports = ["80", "443"]
    protocols = ["tcp"]
  }

  ingress {
    from = ["service.thing2"] # you can specify the name of some other object.
    ports = ["22002..22004"] # ranges supported in this format.
    protocols = ["tcp", "udp"]
  }

  ingress {
    from = ["all"]
    ports = ["all"]
    protocols = ["icmp"]
  }

  ingress {
    from = ["10.0.0.256", "10.0.1.0/24"] # IPs and CIDR blocks supported.
    ports = ["8125"]
    protocols = ["udp"]
  }
}
```

## Motivation

Cloud providers model their security constructs in different ways. For example, some cloud providers allow security group rules to reference other security groups as a means to allow network access. Some do not have this self-referencing capability and instead rely solely on IP ranges or generic IP categories. Because of such differences, we're forced to allow service providers to influence network security design. This can be particularly troublesome if you're building infrastructure that spans many service providers. Being able to generate designs with a single project definition for many service providers without considering the nuances in how the providers model security objects allows us to focus on just what our infrastructure components need.
