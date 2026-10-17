# Container isolation and the scope of a deployment boundary

An isolation claim must name the boundary the platform actually supports. For AWS Fargate, that boundary is the task. Containers inside one task share a network namespace and can communicate over localhost. A sidecar is not another independently isolated Fargate task. [AWS Fargate security considerations](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-security-considerations.html).

The ECS task IAM role is associated with the task, and its permissions are supplied to containers in that task. Assigning one sibling container an empty task role and another a privileged task role is not an ECS task-role configuration. Using separate tasks for differently privileged components, or designing another authenticated credential-delivery mechanism, is an architectural inference from that documented scope. [AWS task IAM roles](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html).

Fargate does not support privileged containers. The only Linux capability customers can add is `CAP_SYS_PTRACE`; a recipe requiring customer-added `CAP_NET_ADMIN` to alter routes or install iptables interception is unsupported. Nor should a design assume arbitrary customer-installed eBPF instrumentation is available. Verify supported platform controls before proposing enforcement. [Fargate task-definition restrictions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-tasks-services.html).

ECS uses `pidMode`. Omitting it gives containers private PID namespaces; `pidMode: task` shares the namespace. Fargate supports that parameter on Linux platform 1.4.0 and later. PID visibility alone does not prove arbitrary memory access, and PID separation alone does not prove separation of credentials, volumes, IPC, or networking. Permissions and tracing controls also matter. [AWS task-definition parameters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html).

The ECS container credential provider uses `AWS_CONTAINER_CREDENTIALS_RELATIVE_URI` with `169.254.170.2`. This is not simply EC2 instance metadata. Deleting an environment variable does not establish that an adversarial process cannot reach a credential route; examine the actual route and authorization. [AWS container credential provider](https://docs.aws.amazon.com/sdkref/latest/guide/feature-container-credentials.html).

AWS documents hardware virtualization between Fargate tasks and has described Firecracker's implementation role. That does not create a microVM boundary between sidecars, protect secrets intentionally delivered to a worker, or prove absence of escapes. State the supported isolation claim and its dependencies. [Fargate and EC2 isolation guidance](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/security-fargate-ec2.html), [AWS Firecracker announcement](https://aws.amazon.com/blogs/opensource/firecracker-open-source-secure-fast-microvm-serverless/).

Exercise: A broker and worker occupy different containers in one Fargate task. Is the broker's more privileged task role isolated from the worker by that placement?

Answer: No. The role is task-scoped. The complete design must establish the worker's actual identity, credential reachability, and allowed communication, rather than infer them from the number of containers.

## Related concepts

[Trusted computing base](trusted-computing-base.md) · [Credential proxy](credential-proxy.md) · [Security evidence](security-evidence.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

The linked AWS documentation defines the platform controls discussed here. These are public platform semantics, not facts about a particular organization's deployment.
