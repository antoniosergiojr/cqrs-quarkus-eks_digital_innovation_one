## Sistema de Orçamento | [Digital Innovation One](https://digitalinnovation.one/)

Implementation to CQRS architecture using Quarkus and Kafka.

## About CQRS - Command Query Responsibility Segregation

According with [Martin Folwer](https://martinfowler.com/bliki/CQRS.html) 
> At its heart is the notion that you can use a different model to update information than the model you use to read information. 
> For some situations, this separation can be valuable, but beware that for most systems CQRS adds risky complexity.

## The application

Simulates a bank account scenario where an end user adds a income or expense transaction, and it is processed in a ascyncronous event sourcing and CQRS architecture to recalculate the user's bank account balance. The user can also request the balance of it's account. Down here you can see the design:

![Design](/images/design.png)

## 📌 Index
- 💻 [Technologies](#-technologies)
- 🚀 [How to run](#-how-to-run)
---

## 💻 Technologies
    - Java
    - Apache Kafka (distributed platform of messaging and streaming. Broker: instances of kafka)
    - Apache ZooKeeper (manage all brokers in kafka)
    - Quarkus (it offers small memory footprint and reduced boot time, indicated only in the scenario of microservices)
    - CQRS architecture (CQRS is an architectural pattern that separates the models for reading and writing data)
    - Docker (set of platform as a service products that use OS-level virtualization to deliver software in packages called containers)
    - Kubernetes (K-ubernate-s = K8s is a open-source system for automating deployment, scaling, and management of containerized applications)
    - Pods Kubernates (represents a single instance of a running process in your cluster)
    - PostgreSQL
    - Amazon EKS (Amazon Elastic Kubernetes Service gives you the flexibility to start, run, and scale Kubernetes applications in the AWS cloud or on-premises)
---
## 🚀 How to run

> Cloning the repository
  ```bash
    # Cloning repository
    git clone https://github.com/antoniosergiojr/cqrs-quarkus-eks_digital_innovation_one.git
  ```

## Deploying the external services

```
docker-compose up -d --build
```
It will deploy four docker containers on your environment with PostgreSQL (or MongoDB), Kafka and Zookepper (required by Kafka)

After deploying Kafka, you'll need to [create the topic on the Kafka cluster](https://kafka.apache.org/quickstart).

## Testing the application

#### Running a CURL request to create a income transaction
```
curl -X POST -H "Content-Type: application/json" -d @income-transaction.json http://localhost:8080/transactions
```
#### Running a CURL request to create a expense transaction
```
curl -X POST -H "Content-Type: application/json" -d @expense-transaction.json http://localhost:8080/transactions
```
#### Running CURL request to fetch the balance
```
curl http://localhost:8081/balance\?accountId\=wesley | json_pp
```
#### Running [K6's](https://k6.io) simple performance test
````
k6 run --vus 10 --duration 60s performance-tests/income.js
k6 run --vus 10 --duration 60s performance-tests/expense.js
````
