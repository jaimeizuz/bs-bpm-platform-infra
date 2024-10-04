# Kogito and Infrastructure services

Compilation order
1. ``mvn clean install bs-rest-services-dto``
2. ``mvn clean install bs-rest-services-client``
3. ``mvn clean install bs-processes-servicetasks``
4. ``mvn clean install bs-processes-bpmn-workflows``
   
There are two execution options:
1. Execute the kogito app in dev mode. It will start the server in localhost:8080, where you can access the management console in http://localhost:8080/q/dev-ui/org.jbpm.jbpm-quarkus-devui/process-instances. This option doesn't include logs&traces:  
    ``mvn "-Dbamoe.build=bamoe-community" clean quarkus:dev -Pdevelopment`` (execute it in the bs-processes-bpmn-workflows folder)

2. Start the docker-compose. It allows you to start the full stack, including logs&traces:  
    ``docker-compose --profile kogito-service --profile kogito-consoles --profile keycloak --profile wiremock --profile messaging-redpanda --profile observability --profile elk up -d``
    
    > IMPORTANT: before starting the compose, first you need to build the kogito bpmn image:  
    > ``mvn "-Dbamoe.build=bamoe-community" "-Pevents" "-Pobservability" "-Pelk-monitoring" clean package -Pcontainer``

To access the docker-compose services:
1. __Kogito Management Console__: [http://localhost:8280/](http://localhost:8280/) (user:jdoe, password:jdoe)
2. __Kogito Tasks Console__: [http://localhost:8380/](http://localhost:8380/) (user:jdoe, password:jdoe)
3. __PostgreSQL database__: [http://localhost:http://localhost:8055/](http://localhost:8055/)
4. __Wiremock studio__: [http://localhost:9000](http://localhost:9000)
5. __Jaeger UI (traces)__: [http://localhost:16686/](http://localhost:16686/)
6. __Kafka events console (based on Redpanda UI)__: [http://localhost:9001/](http://localhost:9001/)
7. __Kibana dashboards__: [http://localhost:5601/](http://localhost:5601/)

To start a process instance, just execute this HTTP request:  
    ``curl -X POST -H 'Content-Type:application/json' http://localhost:8080/ServiceTaskProcess -d '{"expirationTime":"PT30S","countryId":2}'``