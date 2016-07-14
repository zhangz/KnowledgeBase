What exactly is service discovery and registration in SOA?

The concepts are as simple as they sound:
Service Discovery - find the details of connecting to a service by name.
Service Registration - associate the details of connecting to a service by name.

The complexity is in the implementation;
- How do you make sure all systems see a consistent view of this lookup across multiple nodes, even if one or more service registries die.
- How do you make sure services are cleaned up even if they die unexpectedly.
- How do you support multiple services with the same name and how do you distribute the load to each one.

What are they need for?
Service discovery is particularly useful for dynamic configuration. esp in the cloud. A service could change it’s host (possibly port) many times over its life time. The number of instances of a service could change depending on load to provide dynamic scaling.