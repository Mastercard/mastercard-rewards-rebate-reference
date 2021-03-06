# Mastercard Rebates Reference Implementation

[![](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/Mastercard/mastercard-rebates-reference/blob/master/LICENSE)

## Table of Contents
- [Overview](#overview)
  * [Compatibility](#compatibility)
  * [References](#references)
- [Usage](#usage)
  * [Prerequisites](#prerequisites)
  * [Configuration](#configuration)
  * [Integrating with OpenAPI Generator](#integrating-with-openapi-generator)
  * [Build and Execute](#build-and-execute)
- [Use Cases](#use-cases)
- [API Reference](#api-reference)
  * [Authorization](#authorization)
  * [Request Examples](#request-examples)
  * [Recommendation](#recommendation)
- [Support](#support)
- [License](#license)

## Overview <a name="overview"></a>
Mastercard Rebates services is a standalone API that provides clients with the capability of initiating statement credit to a Mastercard card account, independent of the promotion rule and reward scoring. 
This API can be used by the client to handle exception cases with rebate processing or reward scoring. Please see here for more details on the API: [Mastercard Developers](https://developer.mastercard.com/mastercard-rebates/documentation/).

### Compatibility <a name="compatibility"></a>
* [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or later

### References <a name="references"></a>
* [Mastercard’s OAuth Signer library](https://github.com/Mastercard/oauth1-signer-java)
* [Using OAuth 1.0a to Access Mastercard APIs](https://developer.mastercard.com/platform/documentation/using-oauth-1a-to-access-mastercard-apis/)

## Usage <a name="usage"></a>
### Prerequisites <a name="prerequisites"></a>
* [Mastercard Developers Account](https://developer.mastercard.com/dashboard) with access to Mastercard Rebates API
* A text editor or IDE
* [Spring Boot 2.2+](https://spring.io/projects/spring-boot)
* [Apache Maven 3.3+](https://maven.apache.org/download.cgi)
* Set up the `JAVA_HOME` environment variable to match the location of your Java installation.

### Configuration <a name="configuration"></a>
* Create an account at [Mastercard Developers](https://developer.mastercard.com/account/sign-up).  
* Create a new project and add `Mastercard Rebates` API to your project.   
* Configure project and download signing key. It will download the zip file.  
* Select `.p12` file from zip and copy it to `src/main/resources` in the project folder.
* Open `${project.basedir}/src/main/resources/application.properties` and configure below parameters.
    
    >**mastercard.api.base.path=https://sandbox.api.mastercard.com/loyalty/rewards/rebates**, its a static field, will be used as a host to make API calls.
    
    **Below properties will be required for authentication of API calls.**
    
    >**mastercard.api.key.file=**, this refers to .p12 file found in the signing key. Please place .p12 file at src\main\resources in the project folder and add classpath for .p12 file.
    
    >**mastercard.api.consumer.key=**, this refers to your consumer key. Copy it from "Keys" section on your project page in [Mastercard Developers](https://developer.mastercard.com/dashboard)
      
    >**mastercard.api.keystore.alias=keyalias**, this is the default value of key alias. If it is modified, use the updated one from keys section in [Mastercard Developers](https://developer.mastercard.com/dashboard).
    
    >**mastercard.api.keystore.password=keystorepassword**, this is the default value of key alias. If it is modified, use the updated one from keys section in [Mastercard Developers](https://developer.mastercard.com/dashboard).

### Integrating with OpenAPI Generator <a name="integrating-with-openapi-generator"></a>
[OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator) generates API client libraries from [OpenAPI Specs](https://github.com/OAI/OpenAPI-Specification). 
It provides generators and library templates for supporting multiple languages and frameworks.

See also:
* [OpenAPI Generator (maven Plugin)](https://mvnrepository.com/artifact/org.openapitools/openapi-generator-maven-plugin)
* [OpenAPI Generator (executable)](https://mvnrepository.com/artifact/org.openapitools/openapi-generator-cli)
* [CONFIG OPTIONS for java](https://github.com/OpenAPITools/openapi-generator/blob/master/docs/generators/java.md)

#### OpenAPI Generator Plugin Configuration
```xml
<!-- https://mvnrepository.com/artifact/org.openapitools/openapi-generator-maven-plugin -->
<plugin>
    <groupId>org.openapitools</groupId>
    <artifactId>openapi-generator-maven-plugin</artifactId>
    <version>${openapi-generator.version}</version>
    <executions>
        <execution>
            <goals>
                <goal>generate</goal>
            </goals>
            <configuration>
                <inputSpec>${project.basedir}/src/main/resources/Mastercard_Rebates-api-spec.yaml</inputSpec>
                <generatorName>java</generatorName>
                <library>okhttp-gson</library>
                <generateApiTests>false</generateApiTests>
                <generateModelTests>false</generateModelTests>
                <configOptions>
                    <sourceFolder>src/gen/main/java</sourceFolder>
                    <dateLibrary>java8</dateLibrary>
                </configOptions>
            </configuration>
        </execution>
    </executions>
</plugin>
```

#### Generating The API Client Sources
Now that you have all the dependencies you need, you can generate the sources. To do this, use one of the following two methods:

`Using IDE`
* **Method 1**<br/>
  In IntelliJ IDEA, open the Maven window **(View > Tool Windows > Maven)**. Click the icons `Reimport All Maven Projects` and `Generate Sources and Update Folders for All Projects`

* **Method 2**<br/>

  In the same menu, navigate to the commands **({Project name} > Lifecycle)**, select `clean` and `compile` then click the icon `Run Maven Build`. 

`Using Terminal`
* Navigate to the root directory of the project within a terminal window and execute `mvn clean compile` command.

### Build and Execute <a name="build-and-execute"></a>
Once you’ve added the correct properties, we can build the application. We can do this by navigating to the project’s base directory from the terminal and running the following command

`mvn clean install`

When the project builds successfully you can then run the following command to start the project

`java -jar target/mastercard-rebates-reference-1.0.0.jar`

## Use Cases <a name="use-cases"></a>
> Case 1: [REBATE WITH ACCOUNT IDENTIFIER](https://developer.mastercard.com/mastercard-rewards/documentation/use-cases/rebate-with-account-identifier/)
  - This endpoint provides the capability to non PCI certified loyalty providers to offer rebates for all the rewards enrolled accounts mapped to Mastercard generated or client provided account identifiers.
  - Here, the transactions performed by cardholder is scored by Loyalty providers for reward program enrolled accounts and as being the non PCI certified providers would send the transactions using account identifiers to Mastercard to award rebates.
  - Refer to model classes for field level information.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/rebate-transactions` | POST | [RebateTransactions](docs/RebateTransactions.md#account-identifier) | [RebateTransactionResponse](docs/RebateTransactionResponse.md) |

> Case 2: [REBATE WITH DPAN](https://developer.mastercard.com/mastercard-rewards/documentation/use-cases/rebate-with-dpan/)
  - This endpoint provides the capability to loyalty providers to offer rebates for all the Mastercard accounts mapped to DPAN token irrespective of the accounts enrollment into rewards program.
  - Here, the DPAN transactions performed by cardholder is scored by Loyalty providers and rebate processed by Mastercard.
  - Refer to model classes for field level information.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/rebate-transactions` | POST | [RebateTransactions](docs/RebateTransactions.md#dpan) | [RebateTransactionResponse](docs/RebateTransactionResponse.md) |

> Case 3: [REBATE WITH PAN](https://developer.mastercard.com/mastercard-rewards/documentation/use-cases/rebate-with-primary-account-number/)
  - This endpoint provides the capability to loyalty providers to offer rebates for all the Mastercard accounts with PAN irrespective of the accounts enrollment into rewards program.
  - Here, the PAN transactions performed by cardholder is scored by Loyalty providers and rebate processed by Mastercard.
  - Refer to model classes for field level information.

    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/rebate-transactions` | POST | [RebateTransactions](docs/RebateTransactions.md#pan) | [RebateTransactionResponse](docs/RebateTransactionResponse.md) |

> Case 4: [ERROR HANDLING](https://developer.mastercard.com/mastercard-rewards/documentation/api-reference/error-responses/)
  - The operation can fail for various reasons like formatting, field length exceeds, etc.
  - This use case just shows one of the example of such failures.
  - For the complete list of application specific error codes, refer to [Application Error Codes](https://developer.mastercard.com/mastercard-rebates/documentation/api-reference/application-error-codes/).
  - Also refer to model class [Errors](docs/Errors.md) for field level information.
  
## API Reference <a name="api-reference"></a>
To develop a client application that consumes a RESTful Rebates API with Spring Boot, refer below documentation.

| API | Endpoint | HTTP Method | Description |
| :-- | :------- | :---------- | :---------- |
| [Create Rebate Transaction](https://developer.mastercard.com/mastercard-rebates/documentation/api-reference/#apis) | `/rebate-transactions` | POST | This send rebate requests for processing the accounts which may or may not be enrolled into rewards program. |

### Authorization <a name="authorization"></a>
The `com.mastercard.developer.interceptors` package will provide you with some request interceptor classes you can use when configuring your API client. These classes will take care of adding the correct `Authorization` header before sending the request.

### Request Examples <a name="request-examples"></a>
You can change the default input passed to APIs, modify values in following files,
* `com.mastercard.developer.example.RebateTransactionExample.java`

### Recommendation <a name="recommendation"></a>
It is recommended to create an instance of `ApiClient` per thread in a multithreaded environment to avoid any potential issues.

## Support <a name="support"></a>
If you would like further information, please send an email to apisupport@mastercard.com

## License <a name="license"></a>
Copyright 2020 Mastercard
 
Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at
 
       http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
