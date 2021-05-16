# Spring Boot Vault Maven Plugin
**A Spring Boot app to read secrets from HashiCorp Vault using vault maven plugin.**

## Vault Maven Plugin

This Maven plugin extracts secrets from HashiCorp Vault and populates Maven properties.

> **NOTE:** This plugin will only work if the version of KV Secrets Engine is set to 1. If the version of KV Secrets Engine is set to 2 then our application will fail to start because this plugin will throw a 404 error while reading the secrets from the Vault. 
> <br/><br/>This is happening because the response structure has been modified in the case of KV Secret Engine V2. Also, the path structure to read the secrets has been updated in the case of V2. 

### How to Use:
1. Add vault-maven-plugin in pom.xml of your application.
2. Configure the Vault server inside the execution section of the plugin by adding the server URL and Token that will be used to authenticate with the Vault server.
3. Configure the path from where you want to read the secrets.
4. Configure the keys whose values you want to read and assign them to Maven properties.
5. Refer Maven properties inside the application.properties and assign them to the Spring Boot properties.
6. Use Spring Boot properties wherever required in your application.

> **NOTE:** Do not hardcode the vault token inside the pom.xml. You can pass it as an argument while building or running your application.


## How to Run Application

**Before starting the application, make sure:**
1. Vault is up and running on your localhost.
2. You have stored the two secrets with key **username** & **password** in the path **/secrets/v1/dev**. 

> **NOTE:** Inside pom.xml we have stored the path as /secrets/v1/${environment} where the value of environment needs to be passed as an argument while building or running the application.

<br/>

**Start the application using any of the commands mentioned below:**

> **Note:** These commands need to run inside the root folder of this project i.e inside the **spring-boot-vault-maven-plugin** folder.


- **Using maven** <br/>```mvn spring-boot:run -DvaultToken=vaultServerToken -Denvironment=dev```


- **From jar file**<br/>
  Create a jar file using '**mvn clean install -DvaultToken=vaultServerToken -Denvironment=dev**' command and then execute
  <br/>```java -jar target/read-secrets-1.0.1-SNAPSHOT.jar```
  
> **Note:** By default spring boot application starts on port number 8080. If port 8080 is occupied in your system then you can change the port number by uncommenting and updating the **server.port** property inside the **application.properties** file that is available inside the **src > main > resources** folder.

<br/>

**Send an HTTP GET request to '/getSecretsFromVault' endpoint using any of the two methods:**

- **Browser or REST client**
  <br/>```http://localhost:8080/getSecretsFromVault```


- **cURL**
  <br/>```curl --request GET 'http://localhost:8080/getSecretsFromVault```


## How to Run Unit Test Cases

**Run the test cases using any of the commands mentioned below:**

> **Note:** These commands need to run inside the root folder of this project i.e inside the **spring-boot-vault-maven-plugin** folder.

- **To run all the test cases**
  <br/>```mvn test -DvaultToken=vaultServerToken -Denvironment=dev```


- **To run a particular test class**
  <br/>```mvn -Dtest=ReadSecretsControllerTest test -DvaultToken=vaultServerToken -Denvironment=dev```
  <br/>or
  <br/>```mvn -Dtest=ReadSecretsApplicationTests test -DvaultToken=vaultServerToken -Denvironment=dev```

<br/>

> **Note:** While starting your application or while running the maven install command you need to provide the argument -DvaultToken=vaultServerToken -Denvironment=dev or else your application will fail to start / maven install command will also fail due to test case failures.
