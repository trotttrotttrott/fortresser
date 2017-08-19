# Fortresser

Fortresser is a security group and access control list design tool. By defining your infrastructure components and ports used as code, you can generate a design suitable for your security needs on many popular service providers.

It does _not_ actually create objects on your service provider accounts. It only provides a design for you to implement by other means.

## Motivation

Cloud providers model their security constructs in different ways. For example, some cloud providers allow security group rules to reference other security groups as a means to allow network access. Some do not have this self-referencing capability and instead rely solely on IP ranges or generic IP categories. Because of such differences, we're forced to allow service providers to influence network security design. This can be particularly troublesome if you're building infrastructure that spans many service providers. Being able to generate designs with a single project definition for many service providers without considering the nuances in how the providers model security objects allows us to focus on just what our infrastructure components need.
