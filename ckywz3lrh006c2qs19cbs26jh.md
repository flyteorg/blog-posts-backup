## How Opta Makes Deploying Flyte Much Easier

By [Nitin Aggarwal](https://www.linkedin.com/in/nitag/) (Co-Founder/CTO at RunX)

Until very recently, the [instructions](https://docs.flyte.org/en/latest/deployment/aws/manual.html#deployment-aws-manual) to deploy Flyte on AWS required a lot of manual steps, such as creating k8s cluster, configuring complex IAM roles, and getting all the resources to work together. To improve this onboarding process, the Flyte and [Opta](https://www.runx.dev/) contributors came together to solve this using Opta’s new IaC solution. 

![Screenshot 2022-01-27 at 4.52.15 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643282631633/ZFnAdDCAb.png)
*Flyte’s complex infrastructure stack!*

Opta is an Infrastructure as Code (IaC) framework where a user specifies their needs at a high level of abstraction and Opta generates the [Terraform](https://www.terraform.io/)/[Helm](https://helm.sh/) required to achieve these goals. With Opta, users don’t need to be cloud/infrastructure experts as it includes robust, production-ready modules for most cloud services and enables the users to quickly compose their ideal Infrastructure stack.

# Integration Goals
There were a few main goals we wanted to achieve with this integration:

## Production Grade Deployment of Flyte
While it’s super easy to test Flyte using the sandbox deployment which is a single Docker container, a secure, scalable production-grade deployment of Flyte requires a lot more cloud resources. For example, in AWS, it would require setting up:

- K8s Cluster: runs the control plane
- Aurora Postgres: storage for persistent records
- Simple Notification Service (SNS): notifications after completion of a workflow or in the case of abrupt failures
- Simple Queuing Service (SQS): Flyte ships with its own native scheduler, but Flyte can use SQS and Cloudwatch rules optionally for periodic scheduling
- S3: handle core Flyte components such as [Admin](https://github.com/flyteorg/flyteadmin/), [Propeller](https://github.com/flyteorg/flytepropeller), and [DataCatalog](https://github.com/flyteorg/datacatalog); user-runtime containers rely on an object store to hold files
- Fine-grained IAM controls
- DNS for the dashboard

Also, there needs to be some customizability with the deployment based on the size of the workloads and the components that a Flyte user might want to use.

## Multi-cloud
Flyte currently supports AWS, GCP, and Azure deployments via helm charts. However, it still requires some resources (blog storage, IAM, etc.) to be provisioned and an understanding of Flyte’s architecture. The goal is to be able to have a full Flyte deployment in all three big clouds (AWS, Azure, and GCP) via Opta which would make it easier to deploy a scalable Flyte installment.

## Programmatic Creation and Destruction
Folks in the Flyte community also want to be able to create and destroy Flyte deployments programmatically.

# How Opta Helped Achieve the Goals
Opta introduces a new way to do Infrastructure-As-Code where you work with high-level modules, so anyone can build an automated, scalable, and secure infrastructure for their applications without spending weeks or months becoming DevOps experts.

For Flyte’s deployment, the main reasons which make Opta the perfect fit to be the right deployment tool are:

## Configurable Cloud Modules
Opta provides high-level cloud modules which automatically set up core infrastructure elements like secure networking, VPCs, and subnets. It also makes it super simple to connect different modules together in an intuitive manner. For example, Opta makes it super easy to securely pass in database credentials to a Kubernetes deployment.

## AWS/GCP/Azure Integration
Opta supports AWS/GCP/Azure clouds. For common resources like Redis, Postgres, MySQL, Mongo, Kafka, etc., Opta completely abstracts away from the cloud and the same configuration files would work across all clouds. But for more cloud-specific tools like S3, GCS, etc., the Opta contributors worked very closely with the Flyte contributors to add all the functionality needed in AWS and GCP, and are actively working to support Azure.

## Infrastructure as Code (IaC)
As Opta is an IaC solution, it has become easy for the Flyte team to programmatically create and destroy Flyte clusters. In fact, they are using Flyte workflows to manage this pipeline of creating Flyte deployments on demand. 

# Next Steps
Flyte’s Opta integration currently supports AWS and GCP. We are actively working together to bring the same level of ease of deployment to Azure. Though there are no changes required for resources available in all clouds such as Postgres, Redis, managed K8s, resources specific to the cloud-like blob storage services might need to be set up. If you are eager to get that deployed immediately, join our [community](https://slack.opta.dev/) or the Flyte [community](https://slack.flyte.org/), and let us know!
