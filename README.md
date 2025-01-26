# Deploy the Petclinic application 

## To deploy the Petclinic application using the provided files and structure, you can follow these steps. I will outline how to set up the necessary configurations and files for a successful deployment using Docker and Docker Compose.
**Name:** Ebenka christian joel

Step 1: Prepare Your Environment
Ensure you have the following installed on your EC2 server 
Note: you can use the AMI with snapshot ami-01d10c996cef1d0ad
    - Docker
    - Docker Compose
    - Git (to clone the repository if needed)


Step 2: Create the Docker Compose File
Create a docker-compose.yml file to manage the application and its dependencies (e.g., MySQL, Redis)
Note: the folder redis/ has already been created 

```bash

services:
  petclinic:
    build:
      context: .
      dockerfile: Dockerfile.multi
    environment:
      SERVER_PORT: 8080
      MYSQL_URL: jdbc:mysql://mysqlserver/petclinic
    volumes:
      - /app
    ports:
      - "8000:8000"
      - "8080:8080"

  mysqlserver:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: ''
      MYSQL_ALLOW_EMPTY_PASSWORD: 'true'
      MYSQL_USER: petclinic
      MYSQL_PASSWORD: petclinic
      MYSQL_DATABASE: petclinic
    volumes:
      - mysql_config:/etc/mysql/conf.d

  redis-0:
    image: redis:4.0.2
    command: ["redis-server", "/etc/redis/redis.conf"]
    volumes:
      - ./redis/conf/redis-0:/etc/redis/redis.conf

  redis-1:
    image: redis:4.0.2
    command: ["redis-server", "/etc/redis/redis.conf"]
    volumes:
      - ./redis/conf/redis-1:/etc/redis/redis.conf

  redis-2:
    image: redis:4.0.2
    command: ["redis-server", "/etc/redis/redis.conf"]
    volumes:
      - ./redis/conf/redis-2:/etc/redis/redis.conf

  sentinel-01:
    image: redis:4.0.2
    command: ["redis-sentinel", "/etc/redis/sentinel.conf"]
    volumes:
      - ./redis/conf/sentinel-01:/etc/redis/sentinel.conf

  sentinel-02:
    image: redis:4.0.2
    command: ["redis-sentinel", "/etc/redis/sentinel.conf"]
    volumes:
      - ./redis/conf/sentinel-02:/etc/redis/sentinel.conf

  sentinel-03:
    image: redis:4.0.2
    command: ["redis-sentinel", "/etc/redis/sentinel.conf"]
    volumes:
      - ./redis/conf/sentinel-03:/etc/redis/sentinel.conf

volumes:
  mysql_config:
  redis_data:

```

Step 3: Build and Deploy
Navigate to the Project Directory:
Open your terminal and navigate to the project directory where your docker-compose.yml file is located.
Build and Start the Services:
Use the following command to build and run the Docker containers:

- docker-compose up -d
- docker run -p 8080:8080 --name petclinic-app petclinic-docker-petclinic

# important: 

## add "-DskipTests" If you want to speed up the build process and you’re not concerned about running tests at this stage
RUN ./mvnw package -DskipTests
