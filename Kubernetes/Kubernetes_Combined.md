# Kubernetes Combined Notes

## 001.Kubernetes-what-is-container-orchestration.md
✨ **What is Container Orchestration**
- → Container orchestration is the process of managing a cluster of containers while providing various non-functional requirements or services.
- → **Fault Tolerance:** If one container goes down, there should be other containers to back it up.
- → **On-Demand Scaling:**
    - → If the load on a microservice container increases, scaling should happen automatically (more containers created).
    - → When the load decreases, containers should be scaled down to conserve resources.
- → **Auto Discovery:** Microservice applications running on different containers should be able to automatically discover and use each other.
- → **Public Access:** Containers within the cluster need to be accessible by other containers within the cluster, as well as from outside the cluster.
- → **Auto Update and Rollback:**
    - → **Auto Update:** Deploy applications without affecting clients; old containers remain up while new ones come online, then old ones die out.
    - → **Rollback:** Ability to revert to a previous stable cluster state if issues arise with a new deployment.

## 002.Kubernetes-what-is-kubernetes.md
✨ **What is Kubernetes**
- → Kubernetes is a container orchestration tool.
- → It takes containerized applications and quickly creates and maintains a cluster out of them.
- → These clusters can be used for development, testing, and production environments.
- → Can be set up as part of a CI/CD pipeline.
- → Kubernetes clusters can be deployed on organization servers or on the cloud.
- → Maintained by the Cloud Native Computing Foundation (CNCF).
- → Popular cloud environments (Google Cloud, AWS, Azure) have built-in support for Kubernetes, and custom clusters can also be set up on these platforms.

## 003.Kubernetes-what-is-a-pod.md
✨ **What is a Pod**
- → A Pod is the lowest-level object in a Kubernetes cluster.
- → It logically groups related containers together.
- → Defined using the `spec` section within the `template` section of a deployment YAML file.
- → The `spec` section specifies which containers are part of a particular pod.

## 004.Kubernetes-what-is-a-replicaset.md
✨ **What is a ReplicaSet**
- → ReplicaSets manage pods.
- → They specify the desired number of pods to run.
- → Defined in the deployment YAML file under the `spec` section.
- → The `replicas` field within the `spec` section specifies how many pods should be running in the cluster at any given point in time.

## 005.Kubernetes-what-is-a-deployment.md
✨ **What is a Deployment**
- → A Kubernetes Deployment is how pods and ReplicaSets are managed as a single unit.
- → It defines the pods in the `template` section (including volumes and other pod requirements).
- → It also specifies the number of replicas (desired number of pods) in the `replicas` field.
- → All these configurations are managed within a `deployment.yaml` file.

## 006.Kubernetes-what-is-a-service.md
✨ **What is a Service**
- → A Kubernetes Service object logically groups a set of pods that need to communicate with each other and with the outside world.
- → Created using a `service.yaml` file, where the type of service and exposed ports are specified.
- → Services are bound to a particular deployment (which contains the pods) through `labels` and `selectors`.
- → Labels and selectors are used to map a service to a group of pods within a deployment.

## 007.Kubernetes-what-are-different-service-types.md
✨ **Different Service Types in Kubernetes**
- → **ClusterIP:**
    - → Allows pods within a cluster to communicate with each other.
    - → Creates virtual IP addresses for pods using IP tables/IPVS, mapping them to actual pod IP addresses.
    - → Also performs load balancing across pods if they host the same containers.
- → **NodePort:**
    - → Built on top of ClusterIP.
    - → Exposes a higher-order port on each node, allowing applications outside the cluster to communicate with applications running within the cluster.
- → **LoadBalancer:**
    - → Used when configuring an external load balancer (e.g., AWS Load Balancer, Azure Load Balancer).
    - → These external load balancers can balance the load across nodes within a Kubernetes cluster.
- → **ExternalName:**
    - → Used to configure and use an external application (outside the cluster) as if it were inside the cluster.
- → **Ingress (Not a Service Type, but related):**
    - → Used to expose multiple services within the cluster through a single entry point.
    - → Allows using lower port numbers (or actual service port numbers) instead of the higher NodePort numbers.

## 008.Kubernetes-what-are-namespaces.md
✨ **What are Namespaces**
- → Kubernetes Namespaces are used to logically divide a cluster when multiple applications are deployed.
- → They prevent applications from interfering with each other and ensure fair resource allocation.
- → Once a namespace is created and assigned to a project or team, all applications for that project/team should be confined to that namespace.
- → Applications within a namespace can only use the CPU, processes, and other resources allocated to that specific namespace.

## 009.Kubernetes-explain-kubernetes-architecture.md
✨ **Kubernetes Architecture**
- → Kubernetes clusters consist of `nodes`.
- → **Master Nodes:**
    - → **API Server:** The key component that allows the master node to manage the entire cluster. Communication with the API server is done via tools like `kubectl`.
    - → **Scheduler:** Schedules the required pods on the worker nodes.
    - → **Control Manager:** Runs in the background, ensuring the cluster always remains in its desired state (e.g., if a pod goes down, the control manager ensures another comes up).
    - → **etcd:** A distributed key-value store that stores the current state of the cluster. This allows other master nodes to take over if one goes down.
- → **Worker Nodes:**
    - → **Kubelet:** The main component on the worker node, responsible for creating pods and containers inside them. The API server communicates with Kubelet to get work done.
    - → **Kube-proxy:** A network proxy and load balancer present on every worker node. It handles network communication for applications outside the cluster to communicate with containers inside pods, and can also perform load balancing.

## 010.Kubernetes-volumes-vs-pv.md
✨ **Volumes vs Persistent Volumes (PV)**
- → **Volumes:**
    - → Their lifecycle is tied to the pod or node.
    - → If a node dies, the volume is gone.
- → **Persistent Volumes (PV):**
    - → Their lifecycle lasts beyond the scope of a pod or node.
    - → Even if the node dies, the persistent volume will still live.

## 011.Kubernetes-what-are-pv-and-pvc.md
✨ **What are PV and PVC**
- → **Persistent Volume (PV):**
    - → Represents a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes.
    - → It can live as long as the cluster is alive, independent of any specific pod.
- → **Persistent Volume Claim (PVC):**
    - → A request for storage by a user.
    - → It is a request for some space from a Persistent Volume for an application or a group of applications.

## 012.Kubernetes-how-to-use-a-pvc.md
✨ **How to Use a PVC**
- → **Step 1: Create a Persistent Volume (PV):**
    - → Create a YAML file to define the Persistent Volume.
    - → Request the desired capacity on the cluster, specify the host path for storage, and define access modes (e.g., ReadWriteOnce, ReadOnlyMany).
- → **Step 2: Create a Persistent Volume Claim (PVC):**
    - → Create a YAML file for the Persistent Volume Claim per application.
    - → Request the amount of space needed for the application from the Persistent Volume and specify the access mode.
- → **Step 3: Use PVC in Deployment:**
    - → Reference this Persistent Volume Claim in the `volumes` section of your deployment YAML file.
    - → Mount it into your pod just like any other volume, allowing the application to use the persistent storage.

## 013.Kubernetes-what-are-config-maps-and-secrets.md
✨ **What are Config Maps and Secrets**
- → **Config Maps:**
    - → Used to pass configuration information (non-sensitive data) into a Kubernetes cluster.
    - → Define data as key-value pairs.
    - → Can be bound and used within a deployment, similar to how volumes are bound.
- → **Secrets:**
    - → Used for sensitive details like usernames and passwords.
    - → Stored in a temporary location.
    - → Only brought onto the pod when the secret information is required.
    - → Deleted from the pod location as soon as the data is used, but they persist in the temporary location.
