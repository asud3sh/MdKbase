# 22 · CLOUD & EDGE COMPUTING

## 22.1 Cloud Computing

### Cloud Computing Fundamentals

**Definition:** Cloud computing is the on-demand delivery of computing resources (servers, storage, databases, networking, software, analytics, intelligence) over the internet with pay-as-you-go pricing.

**Essential characteristics (NIST):**
1. **On-demand self-service:** Provision resources without human interaction
2. **Broad network access:** Available over network from any device
3. **Resource pooling:** Multi-tenant model; resources dynamically assigned
4. **Rapid elasticity:** Scale up/down quickly
5. **Measured service:** Pay for what you use

**Service models:**

| Model | What you manage | What provider manages | Examples |
|-------|-----------------|----------------------|----------|
| **IaaS** | OS, runtime, apps, data | Virtualization, servers, storage, network | AWS EC2, GCP Compute Engine, Azure VMs |
| **PaaS** | Apps, data | OS, runtime, middleware, infrastructure | Heroku, Google App Engine, Azure App Service |
| **SaaS** | Nothing (just use) | Everything | Gmail, Salesforce, Slack, Office 365 |
| **FaaS** | Functions only | Everything else | AWS Lambda, Cloud Functions, Azure Functions |

**Deployment models:**
- **Public:** Shared infrastructure; AWS, Azure, GCP
- **Private:** Dedicated to one organization; OpenStack, VMware
- **Hybrid:** Mix of public + private
- **Community:** Shared by organizations with common concerns

**Why cloud matters:** Converts capital expenditure (CAPEX) to operational expenditure (OPEX); enables global scale; accelerates innovation.

**Recall:** *Compute as utility.*

### Major Cloud Providers

**AWS (Amazon Web Services):**
- Largest market share
- Services: EC2 (compute), S3 (storage), RDS (database), Lambda (serverless), DynamoDB (NoSQL), SageMaker (ML)
- Regions, Availability Zones, Edge Locations

**Azure (Microsoft):**
- Strong enterprise integration
- Services: VMs, Blob Storage, SQL Database, Functions, Cosmos DB, Azure ML
- Hybrid cloud focus (Azure Arc)

**GCP (Google Cloud Platform):**
- Strong in data analytics and ML
- Services: Compute Engine, Cloud Storage, Cloud SQL, Cloud Functions, BigQuery, Vertex AI
- Kubernetes originated here

**Others:** Oracle Cloud, IBM Cloud, Alibaba Cloud, DigitalOcean, Linode.

**Recall:** *AWS, Azure, GCP.*

### Virtualization in Cloud

**Hypervisor:** Software that creates and runs VMs.
- **Type 1 (bare-metal):** Runs directly on hardware (Xen, ESXi, KVM)
- **Type 2 (hosted):** Runs on host OS (VirtualBox, VMware Workstation)

**VM vs Container:**
| Aspect | VM | Container |
|--------|-----|-----------|
| Isolation | Hardware-level | OS-level |
| Startup | Minutes | Seconds |
| Size | GB | MB |
| Kernel | Separate | Shared |
| Density | Low | High |

**Recall:** *VMs = full isolation; containers = lightweight.*

### Serverless Computing

**Definition:** Run code without managing servers; provider handles scaling.

**FaaS (Function as a Service):**
- Event-driven; stateless
- Auto-scales to zero
- Pay per invocation + duration

**Examples:** AWS Lambda, Google Cloud Functions, Azure Functions, Cloudflare Workers.

**Pros:** No server management, auto-scaling, cost-efficient for spiky workloads.
**Cons:** Cold starts, vendor lock-in, limited runtime.

**Recall:** *No servers to manage.*

### Cloud Storage

**Object storage:** Flat namespace; buckets; unlimited scale.
- **S3:** AWS; buckets; versioning; lifecycle policies
- **GCS:** Google Cloud Storage
- **Azure Blob Storage**

**Block storage:** Volumes attached to VMs.
- **EBS:** AWS Elastic Block Store
- **Persistent Disk:** GCP
- **Azure Disk**

**File storage:** Shared file systems.
- **EFS:** AWS Elastic File System
- **Filestore:** GCP
- **Azure Files**

**Recall:** *Object, block, file.*

### Cloud Networking

**VPC (Virtual Private Cloud):** Isolated network in cloud.

**Subnets:** Public (internet-accessible) vs private.

**Security groups:** Stateful firewall rules.

**Load balancers:** Distribute traffic.

**CDN:** Edge caching for content delivery.

**DNS:** Route 53 (AWS), Cloud DNS (GCP).

**Recall:** *VPC, subnets, security groups.*

---

## 22.2 Edge Computing

### Edge Computing Fundamentals

**Definition:** Process data near its source rather than in a centralized data center.

**Why edge:**
- **Latency:** Reduce round-trip time
- **Bandwidth:** Reduce data transfer
- **Privacy:** Keep data local
- **Reliability:** Work offline
- **Cost:** Reduce cloud egress

**Architecture:**
```
Device → Edge Node → Regional Data Center → Central Cloud
```

**Use cases:** IoT, autonomous vehicles, AR/VR, industrial automation, smart cities, content delivery.

**Recall:** *Move compute to edge.*

### Edge AI & TinyML

**Definition:** Run ML models on edge devices.

**Challenges:**
- Limited compute, memory, power
- Model size constraints
- Real-time inference

**Techniques:**
- **Quantization:** Reduce precision (FP32 → INT8)
- **Pruning:** Remove weights
- **Knowledge distillation:** Train small model from large
- **Neural architecture search:** Find efficient architectures
- **Hardware acceleration:** NPU, DSP, GPU

**Frameworks:** TensorFlow Lite, PyTorch Mobile, ONNX Runtime, Core ML.

**Examples:** Wake word detection, image classification, anomaly detection.

**Recall:** *ML on device.*

### CDNs (Content Delivery Networks)

**Definition:** Geographically distributed servers caching content near users.

**How it works:**
1. User requests content
2. DNS routes to nearest edge server
3. Edge serves cached content or fetches from origin
4. Content cached for future requests

**Benefits:** Reduced latency, offloaded origin, DDoS protection.

**Providers:** Akamai, Cloudflare, Fastly, AWS CloudFront.

**Recall:** *Content near users.*

### 5G MEC (Multi-Access Edge Computing)

**Definition:** Edge computing integrated with 5G networks.

**Benefits:** Ultra-low latency (<10ms), high bandwidth, network slicing.

**Use cases:** Autonomous vehicles, industrial IoT, AR/VR, telemedicine.

**Recall:** *Edge + 5G.*

### Federated Learning

**Definition:** Train ML models across decentralized devices without sharing raw data.

**Process:**
1. Server sends global model to devices
2. Devices train locally on their data
3. Devices send model updates (gradients) to server
4. Server aggregates updates (FedAvg)
5. Repeat

**Benefits:** Privacy, reduced communication, personalization.

**Challenges:** Heterogeneous data, communication cost, security.

**Recall:** *Train locally, aggregate globally.*

---

## 22.3 Cloud-Native Architecture

### Containers & Orchestration

**Docker:** Container runtime; images, containers, registries.

**Kubernetes:** Container orchestration.
- **Pod:** Smallest deployable unit; one or more containers
- **Deployment:** Manages replicas
- **Service:** Stable network endpoint
- **Ingress:** External access
- **ConfigMap/Secret:** Configuration
- **Namespace:** Logical isolation

**Other orchestrators:** Docker Swarm, Nomad, ECS.

**Recall:** *Kubernetes = orchestration.*

### Infrastructure as Code (IaC)

**Definition:** Manage infrastructure through code.

**Tools:**
- **Terraform:** Declarative; multi-cloud
- **Pulumi:** Imperative; multiple languages
- **CloudFormation:** AWS-native
- **Ansible:** Configuration management

**Benefits:** Reproducibility, version control, automation.

**Recall:** *Infrastructure as code.*

### Service Mesh

**Definition:** Infrastructure layer for service-to-service communication.

**Components:**
- **Sidecar proxy:** Per-service proxy (Envoy)
- **Control plane:** Configuration, policy (Istio, Linkerd)

**Features:** Traffic management, security (mTLS), observability.

**Recall:** *Networking layer for microservices.*

### Cloud Security

**Shared responsibility model:** Provider secures cloud; customer secures in cloud.

**IAM (Identity and Access Management):** Users, roles, policies.

**VPC security:** Security groups, NACLs, private subnets.

**Encryption:** At rest, in transit, key management (KMS).

**Compliance:** SOC 2, ISO 27001, HIPAA, GDPR.

**Recall:** *Shared responsibility.*

### Cost Optimization (FinOps)

**Strategies:**
- **Right-sizing:** Match instance to workload
- **Reserved instances:** Commit for discount
- **Spot instances:** Bid for spare capacity
- **Auto-scaling:** Scale with demand
- **Storage tiering:** Hot/warm/cold
- **Serverless:** Pay per use

**Tools:** AWS Cost Explorer, CloudHealth, Kubecost.

**Recall:** *Optimize cloud spend.*

**SOTA:** WebAssembly on edge, 5G MEC, federated learning.

---
