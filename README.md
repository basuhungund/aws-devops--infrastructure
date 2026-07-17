# AWS DevOps Infrastructure Project

A production-style AWS infrastructure built from scratch — networking, compute, container orchestration, auto scaling, load balancing, and monitoring — deploying a Spring Boot application end-to-end.

## 🏗️ Architecture

## 🛠️ Tech Stack

**Cloud:** AWS (VPC, EC2, Auto Scaling Group, Application Load Balancer, CloudWatch, SNS)
**DevOps Tools:** Jenkins, Docker, Maven, Git
**Container Orchestration:** k3s (lightweight Kubernetes)
**Monitoring:** CloudWatch, SNS email alerts

## 📦 What Was Built

1. **Networking:** Custom VPC with public subnets across 2 Availability Zones, Internet Gateway, route tables, and security groups
2. **Compute:** EC2 instance with Java 21, Jenkins, Docker, Maven, and a k3s Kubernetes cluster
3. **Deployment:** Spring Boot application containerized with Docker and deployed to Kubernetes via Deployment and Service manifests
4. **Scaling:** Auto Scaling Group (min 1, max 2) built from a custom AMI, ensuring the infrastructure can recover and scale automatically
5. **Load Balancing:** Application Load Balancer distributing traffic to the Auto Scaling Group, confirmed working end-to-end
6. **Monitoring:** CloudWatch alarm on CPU utilization, connected to an SNS topic for email alerts

**Docker Hub image:** [devops-infra-project:v1](https://hub.docker.com/r/basuhungund/devops-infra-project)

## 🐛 Real Troubleshooting Log

This project involved genuine infrastructure debugging, not just following a tutorial:

1. **Silent config save failures** — subnet and Jenkins sources.list files failed to save on first attempt; fixed by recreating and verifying each resource before proceeding
2. **Security group missing outbound rules** — blocked all internet access from the instance; fixed by adding an Allow All outbound rule
3. **Expired Jenkins signing key** — the 2023 GPG key expired; switched to Jenkins' 2026 key
4. **Java version mismatch** — Jenkins 2.568.1 required Java 21, only Java 17 was installed; installed Java 21 and updated the system default
5. **kubeadm hardware limits** — t2.micro didn't meet kubeadm's minimum CPU/RAM requirements; switched to k3s, a lightweight CNCF-certified Kubernetes distribution
6. **Disk space exhaustion (multiple times)** — Jenkins + Docker + Maven + k3s exceeded the default 8GB volume; diagnosed via `du`/`df`, cleaned orphaned container sandboxes, and ultimately resized the EBS volume to 20GB to permanently resolve it
7. **kubectl misconfiguration** — missing `~/.kube/config` caused connection errors; fixed by re-copying the config directly from k3s
8. **Image visibility across container runtimes** — Docker-built images weren't visible to k3s's separate containerd runtime; fixed using `k3s ctr images import`

## 💰 Cost Optimization Decisions

- Chose k3s over kubeadm to avoid upgrading to a larger, costlier instance type
- Skipped AWS WAF (~$6/month recurring charge) for this portfolio demo — documented how it would be added for production use
- Actively stopped/scaled down resources (Auto Scaling Group capacity, EC2 instance) between work sessions to minimize cost

## 🚀 Getting Started

See `deployment.yaml` and `service.yaml` for the Kubernetes manifests used in this project.

## 📫 Contact

- LinkedIn: [linkedin.com/in/basavaraj-hungund](https://www.linkedin.com/in/basavaraj-hungund-188867371)
- Email: basuhungund07@gmail.com
