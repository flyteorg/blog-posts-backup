## Building a Gateway to Flyte

By: @[Katrina Rogan](@Katrina) and @[Ketan Umare](@ketanumare)

*This [article](https://eng.lyft.com/building-a-gateway-to-flyte-474b451b32c8) was originally published on the Lyft Engineering blog on Nov 19th, 2020.*

At the beginning of the year, we introduced [Flyte](https://eng.lyft.com/introducing-flyte-cloud-native-machine-learning-and-data-processing-platform-fb2bb3046a59), a production-grade, orchestration platform for data and machine learning workflows. Today Flyte has grown to host more than 9,000 workflows, over 20,000 tasks, and over **a million** workflow executions per month on the platform while serving 400+ users (at Lyft). This post dives deep into one of the most critical components of Flyte which has made this possible. Flyte was built at Lyft across multiple iterations and was built from the start to leverage existing open-source systems. However, we realized there was no single open-source project that met our requirements. Some features that were important to us included,

* First-class data awareness and lineage tracking
* Centralized, hosted, and multi-tenant platform for the entire company
* Ability to run disparate workloads written using varied frameworks, that would evolve with the users' needs
* Ability to efficiently maintain, extend and administer Flyte for all our users

Most importantly, we valued: agile development, continuous deployment, and low maintenance overhead for our small team. This was made possible by using a Cloud-native architecture and designing the control plane separately from the data plane. [Flyte Admin](https://github.com/flyteorg/flyteadmin/) is the central stateless service that is the control plane of, and gateway to, Flyte.

This post will dive into the design goals behind Flyte Admin, an architecture overview, and provide a couple of examples of how Flyte Admin has made Flyte extremely adaptable within Lyft.

# Design Goals
**One API**: Flyte Admin hosts the inventory of all the business logic in the company. As a whole, Flyte should be accessible to all users through various modalities like the Console (UI), CLI, programmatic executions and set the foundation for building other differentiated platforms on top of Flyte.

**Decoupled execution and elastic scalability**: We wanted to decouple the data-plane that executes Flyte workflows and tasks from the management and observability of these executions. We also wanted to make sure that we could easily add increased capacity, capabilities, and clusters without any downtime for our users.

**Central management of policies, quotas, and resources**: It was important that any multi-tenant platform we designed provided a mechanism for managing users including generic whitelist/blacklist mechanisms and infrastructure provisioning in one, centralized service.

**Retrieve results and debugging information**: Flyte handles reliability and infrastructure but users still need tools to iterate on their code. Flyte Admin is an easily accessible store for all execution-related data.

**Extensive visibility and observability**: Flyte Admin also powers additional tools for developers on Flyte. These include workflow execution notifications, tracking execution progress in real-time, and monitoring scheduled executions.

# Architecture
Flyte Admin is implemented as a stateless, simple gRPC service, which also surfaces a REST API using the [grpc-gateway](https://grpc-ecosystem.github.io/grpc-gateway/) generator. The service is built with the excellent [gorm](https://gorm.io/index.html) ORM to leverage any relational datastore. Database access patterns are optimized to reduce unnecessary joins and large transactions. Flyte Admin serves as the communications hub for Flyte. It communicates with third-party cloud tools for asynchronous scheduling and notifications, generic blob storage, and storing artifacts in a persistent database.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1620120727191/9Kuv1lKid.png)
*Flyte Admin architecture overview*

Key, implementation features include:

**Persistent, data storage**: User data refers to [tasks](https://docs.flyte.org/en/latest/concepts/tasks.html), [workflows](https://docs.flyte.org/en/latest/concepts/workflows_nodes.html), and their executions. Durably storing this data and exposing it through the single API allows our users to discover and share task definitions and re-use them.

**Reliability and performance**: If an issue affects the data plane, execution progress may be halted but users can still retrieve data artifacts and queue executions for launch. Likewise, if a single, rogue user accidentally hammers the Flyte Admin service this does not affect in-progress executions belonging to other users.

**Centralized resource management**: Flyte Admin ships with the ability to configure a variety of workflow registration and execution parameters. All of these attributes are based on a hierarchical set of tags including user projects and workflows. These *logical* configurations are separate from those of the data plane. This allows Flyte Admin to operate as a single brain, at once aware of its users and capable of modifying the Flyte deployment around them.

**Authentication**: To track the requests to set configuration parameters and other API calls, Flyte Admin ships with support for OpenID Connect authentication and audits all user actions.

# What has this enabled?
Flyte Admin makes it possible to quickly extend the capacity of Flyte by horizontally scaling at a Kubernetes cluster level. This has helped us with:

**Uptime**: We can isolate multiple use cases by criticality. By supporting tiered service levels, we can continue to deploy and enhance Flyte, without affecting the quality of the service for critical use-cases.

**Seamless, runtime updates**: Flyte Admin makes it possible to dynamically and transparently migrate our users onto different execution settings. At Lyft, we’ve scaled Flyte executions across multiple Kubernetes clusters, some of which are dedicated to latency-sensitive clients. Execution placement across these clusters is easily managed by configuration exposed in the Flyte Admin API.

**Cluster bootstrapping**: Adding execution clusters is simple. Flyte Admin creates and syncs common configuration with various Flyte clusters out of the box so that per-user namespaces are configured with no manual intervention. This includes configuring secrets, quotas, service roles, and more. The simple templating system allows Flyte administrators to configure their deployment as they need.

**Single source of truth**: Admin is the centralized repository for execution data. There’s no need for users to hunt down which cluster an execution ran on or where logs have been uploaded.

**Resource arbitrator**: Flyte Admin acts as the centralized gatekeeper for features and resources. This includes quotas to throttle large workloads and tunable options to block unauthenticated workflows from executing.

# Roadmap
Flyte continues to evolve to support not only Lyft but our [partners](https://github.com/flyteorg/flyte#%EF%B8%8F-current-deployments) too. Upcoming Flyte Admin features include:

- **Authorization**: to complement the already existing authentication support.
- **Event Sinks**: Currently workflow notifications are supported on completion for [Email, Slack, and PagerDuty](https://docs.flyte.org/projects/flytekit/en/latest/generated/flytekit.core.notification.html) but we plan to support more generic event sinks including cloud message queues.
- **Granular, priority-based scheduling**: Use a quality of service designation to manage workflow scheduling.
- **Full-fledged multi-cloud support**: Currently Flyte Admin has been tested on AWS and GCP but we plan to incorporate and test Azure support too.

# References and resources
* Previous [blog post](https://eng.lyft.com/introducing-flyte-cloud-native-machine-learning-and-data-processing-platform-fb2bb3046a59) introducing Flyte
* The [Flyte repo](https://github.com/flyteorg/flyte) on Github
* Flyte Admin [control plane overview](https://docs.flyte.org/en/latest/concepts/architecture.html#control-plane) and contributor docs
* Flyte Admin [service how-to](https://docs.flyte.org/en/latest/concepts/admin_service.html)

Cover Image Credits: Photo by [Sam Moqadam](https://unsplash.com/@itssammoqadam) on [Unsplash](https://unsplash.com/photos/DSn1k2kCriA).
