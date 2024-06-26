# Tanzu ContainerApp Demo

## Example Use Case

The environment of the fictional customer has the following requirements:

### Infrastructure Requirements

1. There is a separate network team that is responsible for load balancer configuration.
    * No automatic load balancer configuration is allowed.
    * A ticket must be submitted to configure a load balancer for both internet facing applications and internally available applications.
1. There is a separate team responsible for DNS configuration.
    * No automatic DNS configuration is allowed.
    * A ticket must be submitted to request DNS entries. Wildcard DNS entries are allowed with additional approvals.

### Application Requirements

1. The application which needs to be deployed is managed by a third party vendor.
    * They provide the application as container images which we can't modify or we will be on our own to support them.

## Issues with the Example Use Case

* [ ] There is no ability to define a trait to create a [simple Gateway](./manually_created/example-gateway.yaml)
    * The operations team needs to define a gateway so they can request the load balancer configuration.
* The platform is enforcing unexepected security requirements:
    * [ ] Containers must be non-root
    * [ ] Container filesystem must be read-only

## Configuration & Deployment

Configure the build plan to use ucp (allowing for pre-built images to be used) and specify the registry path to put the containerapp definitions that are created:

```bash
tanzu build config \
  --build-plan-source-type=ucp \
  --containerapp-registry harbor.mbentley.net/mbentley/{name}
```

Deploy the defined `ContainerApp`s:

```bash
tanzu deploy -y
```
