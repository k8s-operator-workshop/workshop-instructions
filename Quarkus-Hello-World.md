Here you will find the different steps to create the 👋 Hello, World! 🌍 Quarkus application.

**Todo**
 - [ ] link the `.gitpod.yml` file

## Requirements

Thanks to GitPod no technical requirements are needed, just copy and past the [.gitpod.yml]() file to your repository and open your repository in Gitpod.  
See the [official documentation](https://www.gitpod.io/docs/getting-started/) of Gitpod to know how to open a project in Gitpod.

## Steps to create the project

ℹ️ The complete source of the project can be found in the [hello-world-from-quarkus](https://github.com/k8s-operator-workshop/hello-world-from-quarkus) repository. ℹ️

### Init and update the project

  - run the following command:

`quarkus create app com.workshop.operator:hello-world-from-quarkus  --extension=resteasy-reactive`

```bash
$ quarkus create app com.workshop.operator:hello-world-from-quarkus  --extension=resteasy-reactive

Looking for the newly published extensions in registry.quarkus.io
-----------
selected extensions: 
- io.quarkus:quarkus-resteasy-reactive


applying codestarts...
📚  java
🔨  maven
📦  quarkus
📝  config-properties
🔧  dockerfiles
🔧  maven-wrapper
🚀  resteasy-reactive-codestart

-----------
[SUCCESS] ✅  quarkus project has been successfully generated in:
--> /Users/sphilipp/dev/talks/2022/workshop-operator/exercices/hello-world-from-quarkus
-----------
Navigate into this directory and get started: quarkus dev
```
  - in the explorer view of VScode the following tree structure was created:
```bash
.
├── README.md
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── docker
    │   │   ├── Dockerfile.jvm
    │   │   ├── Dockerfile.legacy-jar
    │   │   ├── Dockerfile.native
    │   │   └── Dockerfile.native-micro
    │   ├── java
    │   │   └── com
    │   │       └── workshop
    │   │           └── operator
    │   │               └── GreetingResource.java
    │   └── resources
    │       ├── META-INF
    │       │   └── resources
    │       │       └── index.html
    │       └── application.properties
    └── test
        └── java
            └── com
                └── workshop
                    └── operator
                        ├── GreetingResourceIT.java
                        └── GreetingResourceTest.java
```
  - test the generated application by launching the `quarkus dev` command:
```bash
Listening for transport dt_socket at address: 5005
2022-09-15 08:34:33,419 WARN  [io.net.res.dns.DnsServerAddressStreamProviders] (build-5) Can not find io.netty.resolver.dns.macos.MacOSDnsServerAddressStreamProvider in the classpath, fallback to system defaults. This may result in incorrect DNS resolutions on MacOS.
__  ____  __  _____   ___  __ ____  ______ 
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
2022-09-15 08:34:33,771 INFO  [io.quarkus] (Quarkus Main Thread) hello-world-from-quarkus 1.0.0-SNAPSHOT on JVM (powered by Quarkus 2.12.2.Final) started in 0.921s. Listening on: http://localhost:8080

2022-09-15 08:34:33,773 INFO  [io.quarkus] (Quarkus Main Thread) Profile dev activated. Live Coding activated.
2022-09-15 08:34:33,773 INFO  [io.quarkus] (Quarkus Main Thread) Installed features: [cdi, resteasy-reactive, smallrye-context-propagation, vertx]

--
Tests paused
Press [r] to resume testing, [o] Toggle test output, [:] for the terminal, [h] for more options>
```
  - test the greeting end point: `curl http://localhost:8080/hello`
```bash
$ curl http://localhost:8080/hello/
Hello from RESTEasy Reactive
```

> ℹ️ Note that you can use Maven to launch the Quarkus application with the following command: `mvn quarkus:dev` ℹ️

  - update the unit test class `GreetingResourceTest.java` in the `./test/java/com/workshop/operator` as following:
```java
@Test
public void testHelloEndpoint() {
    given()
      .when().get("/hello")
      .then()
          .statusCode(200)
          .body(is("👋  Hello, World ! 🌍"));
}
```
  - in the Quarkus terminal press `r` and show that the test failed:
```bash
2022-09-15 17:56:34,905 ERROR [io.qua.test] (Test runner thread) ==================== TEST REPORT #1 ====================
2022-09-15 17:56:34,905 ERROR [io.qua.test] (Test runner thread) Test GreetingResourceTest#testHelloEndpoint() failed 
: java.lang.AssertionError: 1 expectation failed.
Response body doesn't match expectation.
Expected: is "👋  Hello, World ! 🌍"
  Actual: Hello from RESTEasy Reactive

        at io.restassured.internal.ValidatableResponseImpl.body(ValidatableResponseImpl.groovy)
        at com.workshop.operator.GreetingResourceTest.testHelloEndpoint(GreetingResourceTest.java:18)


2022-09-15 17:56:34,910 ERROR [io.qua.test] (Test runner thread) >>>>>>>>>>>>>>>>>>>> Summary: <<<<<<<<<<<<<<<<<<<<
com.workshop.operator.GreetingResourceTest#testHelloEndpoint(GreetingResourceTest.java:18) GreetingResourceTest#testHelloEndpoint() 1 expectation failed.
Response body doesn't match expectation.
Expected: is "👋  Hello, World ! 🌍"
  Actual: Hello from RESTEasy Reactive

2022-09-15 17:56:34,911 ERROR [io.qua.test] (Test runner thread) >>>>>>>>>>>>>>>>>>>> 1 TEST FAILED <<<<<<<<<<<<<<<<<<<<


--
1 test failed (0 passing, 0 skipped), 1 test was run in 1287ms. Tests completed at 17:56:34.
Press [r] to re-run, [o] Toggle test output, [:] for the terminal, [h] for more options>
```
  - fix the `GreetingResource` class in `./main/java/com/workshop/operator/`:
```java
@GET
@Produces(MediaType.TEXT_PLAIN)
public String hello() {
    return "👋  Hello, World ! 🌍";
}
```
  - see that the test now is ok:
```bash
2022-09-15 17:58:15,459 INFO  [io.qua.test] (Test runner thread) All tests are now passing


--
All 1 test is passing (0 skipped), 1 test was run in 159ms. Tests completed at 17:58:16 due to changes to GreetingResource.class.
Press [r] to re-run, [o] Toggle test output, [:] for the terminal, [h] for more options>
```

## Release and tag the application
  - create the release in Github: see [official documentation](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
  - create the docker image: `docker build -f src/main/docker/Dockerfile.jvm -t <registry name>/hello-world-from-quarkus-jvm:<version> .`, for example `docker build -f src/main/docker/Dockerfile.jvm -t 56hkk1xk.gra7.container-registry.ovh.net/workshop/wilda/hello-world-from-quarkus-jvm:1.0.0 .`
> Note that the docker image version must be the same that the previous created release  
