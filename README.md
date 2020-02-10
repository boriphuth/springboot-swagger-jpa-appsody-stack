# Appsody Stack with Spring® Boot 2, Swagger, JPA

This Appsody Stack, namely `springboot-swagger-jpa`, is inspired by Appsody's Spring Boot 2 Stack, [incubator/java-spring-boot2](https://github.com/appsody/stacks/tree/master/), by Appsodizing [this sample project](https://github.com/brightzheng100/springboot-swagger-jpa-stack).


## Features / Highlights

The Stack, like what Spring Boot 2 Stack does, supports the development of [Spring Boot 2](https://spring.io/projects/spring-boot) applications using OpenJDK 8+ and OpenJ9 from [AdoptOpenJDK](https://adoptopenjdk.net/).

Moreover, it provides a series of integrated capabilities and plugins to streamline and simplify the development of web apps and RESTful APIs.

**The major features:**

- Fully managed by Maven;
- Integrated [Swagger](https://github.com/swagger-api/swagger-ui) so make exposing and documenting RESTful APIs fun;
- Integrated JPA because it's standard and makes talking to RDBMS easy;
- Integrated [H2](https://www.h2database.com) as an embedded database for local development and testing; while you can easily talk to external DB, e.g. MySQL, in production;
- Integrated [Flyway](https://flywaydb.org/) to make database upgrades / patches as code;
- Integrated [micrometer](http://micrometer.io/) to expose Prometheus-style metrics for app monitoring;
- Integrated Spring Boot Test Framework with Junit 4, [Rest Assured](https://github.com/rest-assured/rest-assured) for testing.
- etc.

> Note: Although Maven is provided by the Appsody stack container, allowing you to build, test, and debug your Java application without installing Maven locally, we still recommend installing Maven locally for the best development experience.


## Templates

Templates are used to initialize your application project to quickly start development.

For now, there is only one template named "`default`", which is also the default template of the Stack.

Once initialized, by syntax of `appsody init [<REPO>/]<STACK> [<TEMPLATE>]` , the template will help you scaffold a runnable application with following file structure:

```
$ tree .
.
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── app
    │   │       ├── Application.java
    │   │       ├── config
    │   │       │   └── SwaggerConfig.java
    │   │       ├── controller
    │   │       │   ├── DefaultController.java
    │   │       │   ├── GreetingController.java
    │   │       │   └── StudentController.java
    │   │       ├── model
    │   │       │   ├── Greeting.java
    │   │       │   └── Student.java
    │   │       └── repository
    │   │           └── StudentRepository.java
    │   └── resources
    │       ├── application-default.yml
    │       ├── application-prod.yml
    │       ├── application.yml
    │       ├── bootstrap.yml
    │       └── db
    │           └── migration
    │               ├── V1.0.0__initial.sql
    │               ├── V1.0.1__add_person.sql
    │               └── V1.0.2__alter_student.sql
    └── test
        └── java
            └── app
                └── ApplicationTest.java
```

Where:

- It provides a `pom.xml` file that references the parent POM defined by the Stack;
- Some end-to-end sample code, including JPA-integrated RESTful APIs, is included for developers to reference to;
- The practices of Spring Profiles are embedded;
- A popular way of DB schema upgrades / patches by using `Flyway` is also demonstrated;
- Some test cases are also provided for developers to start with;
- And you may find more.


## Getting Started

### Package Stack

Appsody supports building the Stack from source code, so let's start from scratch.

```sh
$ git clone https://github.com/brightzheng100/springboot-swagger-jpa-appsody-stack.git springboot-swagger-jpa
$ cd springboot-swagger-jpa

$ appsody stack package

$ appsody repo list
NAME      	URL
*dev.local	file:///Users/brightzheng/.appsody/stacks/dev.local/dev.local-index.yaml

$ appsody list dev.local
REPO     	ID                    	VERSION  	TEMPLATES	DESCRIPTION
dev.local	springboot-swagger-jpa	0.1.0    	*default 	A Stack managed by Maven, with Spring Boot, Swagger, JPA
```

Now the stack is ready to use, at `dev.local/springboot-swagger-jpa`.


### Init Application

```
$ cd .. && mkdir my-project && cd my-project

$ appsody init dev.local/springboot-swagger-jpa

$ ls -AL
.appsody-config.yaml .gitignore           .vscode              pom.xml              src
```

This will initialize a Spring Boot 2 project, managed by Maven and itegrated with `Swagger`, `JPA`, `H2`, `Flyway` etc., using the default template.


### Try It Through Using `appsody` Commands

**1. `appsody run`**: To run the application

```sh
$ appsody run
```

You should be able to try out the RESTful APIs exposed and visualized by `Swagger` at:
- http://localhost:8080/swagger/index.html

And Actuator endpoints:
- The health with DB info: http://localhost:8080/actuator/health
- The Flyway info: http://localhost:8080/actuator/flyway
- The Prometheus metrics: http://localhost:8080/actuator/prometheus
- The H2 Console: http://localhost:8080/h2-console

Please refer to [this sample application](https://github.com/brightzheng100/springboot-swagger-jpa-stack) for more details.

**2. `appsody debug`**: To start debugging the application

**3. `appsody test`**: To run test cases for the application

**4. `appsody build`**: To build the Docker image and generate a Kubernetes manifest file

```sh
$ appsody build -t registry:5000/my-project:latest --push
```

This will build a Docker image named `registry:5000/my-project:latest` and push it to Docker registry at `registry:5000`.
Do change it accordingly for your env.

**5. `appsody deploy`**: To deploy it to Kubernetes.

```sh
$ appsody deploy -t registry:5000/my-project:latest
```

> Note: by default, `appsody deploy` will include the scope of `appsody build` at least as of current Appsody releases (e.g. v0.5.x). Adding flag `--no-build` can ignore the build process.

**6. `kubectl get`**: To check it out from Kubernetes.

```sh
$ kubectl get appsodyapplication,pod
NAME                                        IMAGE                             EXPOSED   RECONCILED   AGE
appsodyapplication.appsody.dev/my-project   registry:5000/my-project:latest   true      True         6m45s

NAME                                   READY   STATUS    RESTARTS   AGE
pod/appsody-operator-595d74b67-l8qhk   1/1     Running   0          6m46s
pod/my-project-d69489999-r7mtd         1/1     Running   0          6m26s
```

So it works end to end. Enjoy!


## License

This Stack is licensed under the [Apache 2.0](./image/LICENSE) license.