# Install Docker on AWS EC2
* sudo yum install docker 
* sudo service docker start
* sudo usermod -a -G docker ec2-user

docker inspect id 

# Run RabbitMQ Docker Container
* docker run -d --name rabbit-name-management -p 15672:15672 -p 5672:5672 -p 5671:5671 rabbitmq:3-management

## To run RabbitMQ and change Default user name and password:

docker run -d --name rabbit-name-management -p 15672:15672 -p 5672:5672 -p 5671:5671 -e RABBITMQ_DEFAULT_USER=user –e RABBITMQ_DEFAULT_PASS=password rabbitmq:3-management



# Run Config Servere Docker Container
docker run -d -p 8012:8012 -e "spring.rabbitmq.host=172.17.0.2" kargopolov/config-server



# Run Eureka Docker Container
docker run -d -p 8010:8010 -e "spring.cloud.config.uri=http://172.31.0.133:8012" kargopolov/sk-eureka-server



# Run Zuul API Gateway Docker Container
docker run -d -e "spring.cloud.config.uri=http://99.79.172.54:8012" -e "spring.rabbitmq.host=99.79.172.54" -p 8011:8011 kargopolov/sk-zuul-api-gateway



# Run Elasticsearch Docker Container
docker run -d --name elasticsearch --network api_network -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.3.0


# Run Kibana Docker Container
docker run -d --network api-network -p 5601:5601 kibana:7.3.0

join kibana with elastich search
docker run -d --link elasticsearch:ealistsearch -p 5601:5601 kibana:7.3.0

same network
docker network create --driver bridge api-newtwork
docker run -d --network api-network -p 5601:5601 kibana:7.3.0


# Run Albums Microservice Docker Container
docker run -it -d -e "eureka.client.serviceUrl.defaultZone=http://test:test@99.79.99.76:8010/eureka" --network host -e "logging.file=/api-logs/albums-ws.log" -v /home/ec2-user/api-logs:/api-logs kargopolov/albums-microservice



# Run Logstash for Albums Microservice Docker Container
docker run -d --name logstash /home/ec2-user/api-logs:/api-logs kargopolov/sk-albums-microservice-logstash



# Run MySQL Docker Container
]* docker run –d -p 3306:3306 --name mysql-docker-container -e MYSQL_ROOT_PASSWORD=mauro -e MYSQL_DATABASE=photo_app -e MYSQL_USER=mauro -e MYSQL_PASSWORD=mauro mysql:latest
## for persist data in ec2 instance 
* docker run –d -p 3306:3306 --name mysql-docker-container -e MYSQL_ROOT_PASSWORD=mauro -e MYSQL_DATABASE=photo_app -e MYSQL_USER=mauro -e MYSQL_PASSWORD=mauro -v /var/lib/mysql:/var/lib/mysql mysql:latest

# Run Users Microservice Docker Container
docker run -d -e "spring.cloud.config.uri=http://172.31.4.43:8012" -e "spring.rabbitmq.host=172.31.4.43" -e "eureka.client.serviceUrl.defaultZone=http://test:test@172.31.18.99:8010/eureka" -e "spring.datasource.url=jdbc:mysql://172.31.13.167:3306/photo_app?serverTimezone=UTC" --network host -e "logging.file=/api-logs/users-ws.log" -v /home/ec2-user/api-logs:/api-logs kargopolov/sk-users-microservice



# Run Logstash for Users Microservice
docker run -d --name users-ws-logstash /home/ec2-user/home:/api-logs kargopolov/sk-users-ws-logstash
