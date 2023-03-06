# :blush: UX Design :coffee:
**************************************************
## Requirements:
* Java
* Springboot
* Maven
* PostgreSQL
* POSTMAN
* Minukube/Kubernetes
* IntelliJIDEA Ultimate Edition --only the Ultimnate Edition has fully support for springboot application

## Background technology:
### Springboot
![PIC1](https://www.openxcell.com/wp-content/uploads/2022/09/What-are-the-Main-Features-of-Spring.jpg)
A Java-based framework used for building web applications and services. It provides a set of tools and features that make it easier to develop, test, and deploy applications.

### JAVA
![PIC2](https://www.oracle.com/oce/press/assets/CONT2F6AE229113D42EC9C59FAED5BAA0380/native/og-social-java-logo.gif)
A high-level, object-oriented programming language that is commonly used for developing enterprise applications. It can run on multiple platforms, including Windows, Mac, and Linux.

### Maven
![PIC3](https://cdn.weareneon.com/app/uploads/2016/02/26122924/2000px-Maven_logo.svg_-1296x675-min-1024x533.jpg)
A build automation tool used for building and managing Java projects. It provides a set of plugins and dependencies that simplify the development process and automate common tasks.

### PostgreSQL
![PIC5](https://audviklabs.in/wp-content/uploads/2022/01/postgreSQL.png)
An open-source relational database management system that is widely used for storing and managing data. It provides features such as ACID compliance, transactions, and foreign keys.

### POSTMAN
![PIC6](https://mms.businesswire.com/media/20210818005151/en/761650/23/postman-logo-vert-2018.jpg)
A popular API development tool used for testing and documenting APIs. It provides a user-friendly interface for making requests to APIs and inspecting the responses.

### Minikube
![PIC7](https://codecrux.com/img/blogs/minikube.jpeg)
A tool used for running Kubernetes locally. It allows developers to test and experiment with Kubernetes deployments on their local machine without needing a full-scale production environment.

## Kubernetes
![PIC8](https://i0.wp.com/cloudwithease.com/wp-content/uploads/2022/09/what-is-kubernetes-dp.jpg)
An open-source platform used for deploying, scaling, and managing containerized applications. It provides features such as automatic scaling, load balancing, and self-healing.

## IntelliJIDEA Ultimate Edition
![PIC9](https://media.geeksforgeeks.org/wp-content/uploads/20190501124658/setting1.png)
[IntelliJ IDEA](https://www.jetbrains.com/idea/download/#section=linux) is an integrated development environment (IDE) designed primarily for Java development. It is developed by JetBrains and is available as both a community edition and an ultimate edition with additional features.

IntelliJ IDEA provides a wide range of features to enhance developer productivity, including:

    * Code completion and inspection
    * Code refactoring
    * Debugging and testing tools
    * Version control system integration
    * Built-in support for various web frameworks and libraries
    * Integration with build tools like Maven, Gradle, and Ant
    * Database tools for managing and querying databases
    * Support for multiple programming languages, including Java, Kotlin, Python, and more.

The ultimate edition of IntelliJ IDEA includes additional features such as support for advanced frameworks like Spring, Play, and Hibernate, and support for multiple application servers. It also includes additional tools for working with databases, web services, and mobile development.

Overall, IntelliJ IDEA is a powerful and versatile IDE that can be used for a wide range of development tasks.

## Set up for Springboot:
[Springboot](https://start.spring.io/)

Dependencies selection:
* Spring WEB
* Spring Driver PostgreSQL
* Spring WEB JPA

Requirements:
* Snapshot 3.0.4
* Java 17
* Maven

## Create a secure connection with PostgreSQL using Minikube
### Pgadmin4 installation
[Link](https://www.pgadmin.org/download/)

### Minikube & Kubernetes installation
For Windows 
[Link](https://www.getambassador.io/resources/application-developers-guide-to-setting-up-kubernetes-with-minikube-on-windows-home)

For Linux 
[Link](https://docs.altinity.com/clickhouseonkubernetes/kubernetesinstallguide/minikubeonlinux/)

For MAC
[Link](https://matthewpalmer.net/kubernetes-app-developer/articles/guide-install-kubernetes-mac.html)

### Kubernetes && PostgreSQL 
#### The 1st connection set up:
Create a postgresql config in any directory to enable connection.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
spec:
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres
          env:
            - name: POSTGRES_DB
              value: accountservice
            - name: POSTGRES_USER
              value: your-own-username
            - name: POSTGRES_PASSWORD
              value: yor-own-password
          ports:
            - containerPort: 5432
              name: postgres
---
apiVersion: v1
kind: Service
metadata:
  name: postgres
spec:
  selector:
    app: postgres
  ports:
    - name: postgres
      port: 5432
      targetPort: 5432

```

This is a example of connection to PostgreSQL. The PostgreSQL tree should look like:

```
Server
    Demo
        Database
            accountservice
            database_sth
            todolist
            cars
            ...
```

Save the file as "postgres.yaml" and export it with this command:

```
kubectl apply -f postgres.yaml
```

#### Connect Minikube to PostgreSQL:
Start minikube with the docker driver using the following command:

```
minikube start --vm-driver=docker
```

Connect to the PostgreSQL database using psql. First, get the name of the pod running the PostgreSQL database by running the following command:

```
kubectl get pods
```
This will show you a list of all the pods in your Kubernetes cluster. Look for the pod that is running the PostgreSQL database.


Connect to your PostgreSQL database using psql. You can do this by running the following command:

```
kubectl exec -it <pod_name> -- psql -U <username> <database_name>
```
Replace <pod_name> with the name of the pod running your PostgreSQL container, <username> with the username for your PostgreSQL database (in your case, postgres), and <database_name> with the name of the database you want to connect to (in your case, accountservice).


Testing the connection:
Once you are connected to your PostgreSQL database, you can create a new table by running a CREATE TABLE command. For example, you can create a table called users with columns for id, name, and email like this:

```
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT
);

```

As if Minikube is sucessfully connected to PostgreSQL. Then you will be able to view this latest edit on [Pgadmin4 page](https://127.0.0.1/pgadmin4/)

### Starting the application with IntelliJIDEA
#### properties Settings :
Remember to make a config file for the database connection properties in your application.properties file:

```
spring.datasource.url=jdbc:postgresql://localhost:5432/accountservice
spring.datasource.username=postgres
spring.datasource.password=you-own-password

```
Replace the url, username, and password values with your own PostgreSQL database connection details.

Everything is ready. Happy coding :heart: :zap: