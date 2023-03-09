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

</br>

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

</br>

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
</br>
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

</br></br></br>

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

Save the file as "postgres.yaml" , then run the minikube & export the yaml file with this command:
Start minikube with the docker driver using the following command:

```
minikube start --vm-driver=docker
```


export the yaml file with this command
```
kubectl apply -f postgres.yaml
```

</br></br></br>

#### Using Minikube to create a connection to PostgreSQL :
##### Connecting to a existed database which can be viewed on pgadmin4:
###### Option 1:

Connect to the PostgreSQL database using psql. First, get the name of the pod running the PostgreSQL database by running the following command:


You can use the following commands to check the status of your deployment and service:
```
kubectl get deployment postgres
kubectl get service postgres
```

If the deployment and service are running correctly, you should see output similar to the following:

```
$ kubectl get deployment postgres
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
postgres   1/1     1            1           2d

$ kubectl get service postgres
NAME       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
postgres   ClusterIP   10.100.42.197   <none>        5432/TCP   2d

```

If the service is of type ClusterIP, you won't be able to connect to it directly from outside the cluster. Instead, you need to use a port-forwarding command to forward traffic from your local machine to the service. You can do this with the following command:

```
kubectl port-forward service/postgres 5432:5432
```

This will forward traffic from port 5432 on your local machine to port 5432 on the postgres service in the cluster. Once this is done, you should be able to connect to the PostgreSQL server using psql on your local machine:

```
psql -h localhost -p 5432 -U postgres -d accountservice

```

</br>
Note that you can not try this:

Connect to your PostgreSQL database using psql. You can do this by running the following command:

```
kubectl exec -it <pod_name> -- psql -U <username> <database_name>
```
<i><red>Because: you cannot use kubectl exec to connect to the PostgreSQL server like you did with psql -h localhost -p 5432 -U postgres -d accountservice. kubectl exec allows you to execute a command inside a container running in a pod, but it doesn't provide the network connection necessary to reach the PostgreSQL server.

Instead, you should use kubectl port-forward to forward traffic from your local machine to the PostgreSQL service running in the Kubernetes cluster.</red></i>

</br></br>

###### Option2:

Make the yaml file and applyit :

```
machine@1666:~$kubectl apply -f postgres-deployment.yaml 
deployment.apps/postgres configured
service/postgres unchanged

```

Then check the pods and services:
```
machine@1666:~$kubectl get pods
NAME                        READY   STATUS              RESTARTS      AGE
postgres-748966b777-djnp7   0/1     ContainerCreating   0             8s
postgres-9bfb44bb6-l9p7r    1/1     Running             1 (32h ago)   39h
machine@1666:~$ kubectl get service
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          14d
postgres     NodePort    10.99.186.145   <none>        5432:30470/TCP   13d

```
Then connect to the existed databse with this comamnd:

```
psql -h localhost -p 5432 -U postgres -d accountservice
```
</br></br></br>

##### Connecting to a totally new database inside the cluster:

```
kubectl run -it --rm --image=postgres --restart=Never postgres-client -- psql postgresql://postgres:your-own-password@postgres:5432/your-db-name
```

For special characters, you can try with this:

```
kubectl run -it --rm --image=postgres --restart=Never postgres-client -- psql postgres://postgres:'@@_%11newpassword'@postgres:5432/accountservice

```


OR:

```
machine@1666:~$ kubectl get pods
NAME                        READY   STATUS              RESTARTS   AGE
postgres-5f6d854bb8-dwbhk   1/1     Running             0          37s
postgres-client             0/1     ContainerCreating   0          12s

machine@1666:~$kubectl exec -it postgres-5f6d854bb8-dwbhk -- psql -U postgres -d accountservice
```

</br>

In this command, 'postgres' is the name of the Docker 'image' for the official PostgreSQL container. 
</br>

'your-own-passowrd' is the password you specified when you created the PostgreSQL deployment and 'your-db-name' is the name of the database you want to connect to. 
</br>

You can replace these values with your own credentials and database name as needed.
</br></br>

<i>Alternatively: 
With 'kubectl run' command to create a new pod with the Postgres client image and connect to a new, empty database, any data you add to that database will be lost when the pod is deleted.

When you restart Kubernetes, the pod created by the kubectl run command will be deleted, and a new pod will be created based on the configuration in your deployment yaml file. 

If you have not configured your deployment to use a persistent volume to store the database data, any changes you made to the database will be lost and the database will be empty again.
</i>

</br></br></br>

##### Testing the connection:

Once you are connected to your PostgreSQL database, you can create a new table by running a CREATE TABLE command. 

For example, you can create a table called users with columns for id, name, and email like this:

```
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT
);

```

As if Minikube is sucessfully connected to PostgreSQL. Then you will be able to view this latest edit on [Pgadmin4 page](https://127.0.0.1/pgadmin4/)

</br></br></br>

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
