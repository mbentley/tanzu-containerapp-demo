# Tanzu ContainerApp Demo

## Example Use Case

The environment of the fictional customer has the following requirements:

### Infrastructure Requirements

1. The platform team is able to deploy Avi (NSX ALB) for use within their private network but it doesn't extend beyond the private network.
    * `<client>` -> `<network team LB>` -> `<platform team Avi LB>` -> `<k8s ingress/svc>`
1. There is a separate network team that is responsible for load balancer configuration to go outside of the private network.
    * No automatic load balancer configuration is allowed on the load balancers that traffic must go through to get out of the private datacenter networks
    * A ticket must be submitted to configure a load balancer for both internet facing applications and internally available applications.
1. There is a separate team responsible for DNS configuration.
    * No automatic DNS configuration is allowed.
    * A ticket must be submitted to request DNS entries. Wildcard DNS entries are allowed with additional approvals.

### Application Requirements

1. The application which needs to be deployed is created and managed by a third party vendor.
    * They provide the application as container images which we can't modify or we will be on our own to support them.
    * They typically provide k8s deployment YAML files for running the application

## Issues with the Example Use Case

* [ ] There is no ability to define a trait to create a [simple Gateway](./manually_created/example-gateway.yaml)
    * The operations team needs to define a gateway so they can request the external load balancer configuration.
* [ ] Platform security enforcement: Containers must be non-root
    * Modification of the images from the vendor may not be supported
* [ ] Platform security enforcement: Container filesystem must be read-only
    * Changing the application to make this work may not be supported
* [ ] Image pull policy is `IfNotPresent`
    * Unable to modify this behavior; may lead to unexpected behavior if only using image tags and not digests

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
