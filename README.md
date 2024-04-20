# ealing with Toil at UpCommerce

At last week's UpCommerce SRE team stand-up meeting, two significant topics emerged as sources of toil:

1. Swype.com Downtime: UpCommerce, relying on Swype.com as its payment gateway provider, faced significant downtime. During these periods payments in queue did not roll back, leading to customer claims against UpCommerce. The Engineering Lead tasked the SRE team with preventing such incidents from recurring. To address this issue, two solutions were proposed:

- Code Refactoring: The team proposed pulling out the payment system implementation from UpCommerce's original implementation and transforming it into a microservice. The proposed implementation details are already captured in the swype-deployment.yml and swype-service.yml files. 

- Fail-Safe Mechanism: Another solution involves implementing a "big red button" service. This service would shut down Swype's payment service in UpCommerce's cluster if it exhibited erratic behavior.

2. Ticketing System Challenges: The ticketing system for alert management had been a major point of contention among engineers. Complaints included recurring obsolete issues and a lack of clear prioritization for incoming issues. An example of the ticketing system's output is as follows:
```
Zone XQ: EndpointRegistrationTransientFailure
Zone OH-1: EndpointRegistrationTransientFailure
Zone OH-1: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone OH-1: EndpointRegistrationTransientFailure
Zone OH-1: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone AB: EndpointRegistrationTransientFailure
Zone AB: LLMBatchProcessingJobFailures
Zone AB: EndpointRegistrationFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone OH-1: EndpointRegistrationTransientFailure
Zone OH-1: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone AB: EndpointRegistrationTransientFailure
Zone AB: LLMBatchProcessingJobFailures
Zone AB: TooFewEndpointsRegistered
Zone AB: LLMModelStale
Zone OH-1: EndpointRegistrationTransientFailure
Zone OH-1: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone XQ: EndpointRegistrationTransientFailure
Zone AB: EndpointRegistrationTransientFailure
Zone AB: LLMBatchProcessingJobFailures
Zone AB: EndpointRegistrationFailure
Zone AB: LLMModelVeryStale
```
# Getting your development environment ready

For this week's project you will use GitHub Codespaces as your development environment, just like you did for the projects in previous weeks.

# Steps

1. Create a fork of the week's repository.

2. When you have created a fork of the week's repository, start a codespace on the main branch.

3. Run the command below in your codespace's terminal to create a single-node, Kubernetes cluster using Minikube: 
```
minikube start
```
4. Once your Minikube cluster is running, enter the command below:
```
kubectl create namespace sre
```
This creates a namespace in your Kubernetes cluster named sre. It is within this namespace that you'll do all the tasks required for this project.

5. Run the command below to create the required resources for this project

```
kubectl apply -f upcommerce-deployment.yml -n sre
kubectl apply -f swype-deployment.yml -n sre
```

