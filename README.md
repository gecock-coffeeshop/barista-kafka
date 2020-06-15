# Coffee Shop demo barista-kafka service

This microservice provides a event-based implementation of a barista for the Coffee Shop demo using Quarkus and Kafka. The barista receives requests for coffee by subscribing to the `orders` topic. It fulfils them asynchoronously and publishes to the `queue` topic. For more information on the Coffee Shop scenario, see [Design and deliver an event-driven, cloud-native application at lightning speed](https://developer.ibm.com/tutorials/accelerator-for-event-driven-solutions/).

## Local development

This project has been developed using [Appsody](https://appsody.dev/).  This service needs to connect to a Kafka broker, so to develop locally you must start one.  You can do this using the providing Docker Compose file, with the following command:
```
docker-compose up
```
Now run the command `docker network list`, you should see a new network with the name of your project directory and the word `_default` appended. For example, `barista-kafka_default`.

You can now start the application locally, inside a Docker container, using the following command:
```
appsody run --network barista-kafka_default --docker-options "--env KAFKA_BOOTSTRAP_SERVERS=kafka:9092"
```

For more information, see [Local development](https://appsody.dev/docs/using-appsody/local-development). 

## Building and deploying to a Kubernetes cluster

You can use Appsody to deploy the service to a Kubernetes cluster.
Edit `app-deploy.yaml` to override the bootstrap server configuration by setting an enviornment variable as follows:

```
spec:
  env:
    - name: KAFKA_BOOTSTRAP_SERVERS
      value: <your-kafka-host>:<port>
```  


Now issue the command `appsody deploy`. This will build a Docker image suitable for production, and deploy it by creating the necessary resources on your Kubernetes cluster. For more information, see [Deploying](https://appsody.dev/docs/using-appsody/deploying).  

You can also use the [GitOps repo](https://github.com/ibm-icpa-coffeeshop/gitops-dev) to deploy the complete application. This includes provisioning a Kafka cluster using the Strimzi operator.