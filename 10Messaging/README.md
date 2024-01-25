
We can refresh a project by using ```/actuator/refresh``` endpoint - however this only refreshes the project for one endpoint at the particular port. A cloud bus allows a refresh of different isntances of a microservices for one endpoint.

## Using Custom Properties

We can specify custom values in our application.properties file like so.
**product-service.properties**
```
com.affiq.springcloud.prop=local1
```

We can then inject this into our program, so for example through the ProductController.
**ProductController.class**
```
    @Value("${com.affiq.springcloud.prop}")
    private String prop;

    @GetMapping("/prop")
    public String getProp() {
        return this.prop;
    }
```

## Enabling Refresh Endpoints
To expose all endspoints for spring actuator, we will simply need to add an additonal property in our product-service.class file - ```management.endpoints.web.exposure.include=*```. We will also need to add the ```@RefreshScope``` annotation so that during runtime if the property changes, it can be refreshed.

## Testing the Custom Value endpoint
We can then access the new endpoint through ```http://localhost:9090/productapi/prop```.

We can then change our custom value in the properties file, and then need refresh this using Postman.

## Enabling Distributed Refreshes
We will now introduce a Cloud Bus as a middleman - all microservice instances will register with the cloud bus. One instance will then trigger a refresh through the ```bus/refresh``` endpoint through a message broker (RabbitMQ or Kafka).

## RabbitMQ
We will first need to install RabbitMQ and then use Cmd to navigate to the sbin directory.
```cd "C:\Program Files\RabbitMQ Server\rabbitmq_server-{version}\sbin"``` before running the bat file using ```rabbitmq-server.bat```.

## Adding Dependency
We will then need to add the Spring Cloud bus starter dependency to the Product Service with the AMQP protocol. We will then need to go to the bootstrap.properties file and add these configurations.

## Testing RabbitMQ
We will then need to spin up multiple instances by changing the port number of the product-service.properties file


