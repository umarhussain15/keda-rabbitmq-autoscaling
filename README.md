# RabbitMQ Consumer Autoscaling with KEDA

This repository contains a setup for autoscaling RabbitMQ consumers using 
[KEDA (Kubernetes Event-Driven Autoscaling)](https://keda.sh/). The autoscaling is based on the queue metrics, 
allowing RabbitMQ consumers to scale dynamically depending on the number of messages in the queue.

## Features

- **KEDA Autoscaling**: Automatically scale RabbitMQ consumers based on queue metrics.
- **RabbitMQ Operator**: Manage RabbitMQ clusters easily on Kubernetes.
- **Producer and Consumer Applications**: Go applications that simulate message producers and consumers.
- **Taskfile Automation**: Use [Taskfile](https://taskfile.dev) to manage and automate project tasks.
- **Kind Cluster**: Local Kubernetes cluster using [Kind](https://kind.sigs.k8s.io/).

## Prerequisites

Before running the project, ensure that you have the following installed:

- [Docker](https://docs.docker.com/get-docker/)
- [Kind](https://kind.sigs.k8s.io/)
- [Task](https://taskfile.dev/installation/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [helmfile](https://helmfile.readthedocs.io/en/latest/)

## Getting Started

To set up the project locally, you can use the `start-stack` task from the `taskfile.yml` to automate the setup process.

### Step 1: Clone the Repository

```bash
git clone https://github.com/umarhussain15/keda-rabbitmq-autoscaling.git
cd keda-rabbitmq-autoscaling
```

### Step 2: Start the Stack
Run the following command to set up the Kubernetes environment with a Kind cluster, install RabbitMQ, build the Docker 
images for the producer and consumer applications, and load them into the Kind cluster.

```bash
task start-stack
```

This command will:

1. Set up a Kind Kubernetes cluster locally.
2. Install the RabbitMQ Operator.
3. Deploy a RabbitMQ server cluster on the Kind Kubernetes cluster.
4. Build the Go-based producer and consumer applications as Docker images.
5. Load the Docker images into the Kind cluster.
6. Deploy the helm charts for the applications in the cluster

### Step 3: Verify the Setup

You can verify that everything is running correctly by checking the status of the RabbitMQ cluster and KEDA.

```bash
task use-kind-cluster-context
kubectl get all -n rabbitmq-system
kubectl get scaledobjects
```

### Step 4: Producing and Consuming Messages
Once the stack is running, the producer will start sending messages to the RabbitMQ queue, and the consumers will be automatically scaled based on the metrics from the queue.

You can observe the consumer autoscaling in action using the following command:

```bash
kubectl get hpa
```

### Step 5: Cleanup

To delete the Kind cluster and clean up all resources, run:

```bash
task clean
```

## Controlling the message rates in producer and consumer

You can control the message rates in `helm/keda-rabbitmq/values.yaml` file under `consumer` and `producer` objects. Then
rerun the `task start-stack` so that `helmfile` can sync the changes.

## References
* [KEDA Documentation](https://keda.sh/docs)
* [RabbitMQ Kubernetes Operator](https://www.rabbitmq.com/kubernetes/operator/operator-overview)
* [Kind Documentation](https://kind.sigs.k8s.io/)

Disclaimer: Most of the README was generated by using ChatGPT.