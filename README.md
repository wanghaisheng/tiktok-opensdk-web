# tiktok-opensdk-web

https://github.com/sns-sdks/python-tiktok

# Introduction
An SDK built to facilitate application development to integrate with TikTok developer API.

https://developers.tiktok.com/

## ios/android sdk

official  sdk for android and ios is here

https://github.com/tiktok/tiktok-opensdk-android

https://github.com/tiktok/tiktok-opensdk-ios

but there is none sdk for web, especially we want to run our code on remote server or integrate tiktok openapi with our BS applications


##  methology


insprided from https://github.com/tiktok/tiktok-business-api-sdk/tree/main/python_sdk,

Comprehensive collection of client libraries that enable our developers to build software to integrate with [Tiktok OpenAPI](https://developers.tiktok.com/) faster and in a more standardized way.

* First we use chatgpt to convert offical documentation into swagger json file 

* We generate different programming language  packageautomatically by the Swagger Codegen project




https://github.com/OpenAPITools/openapi-generator






## gen swagger json from doc description 

https://developers.tiktok.com/

years ago, we need manually input and edit,    

now chatgpt can help this 


## gen python java rust sdk from swagger json

https://github.com/swagger-api/swagger-codegen

The openapi-generator-cli tool supports generating code for a wide range of programming languages. Here are some of the programming languages that can be generated using openapi-generator-cli:

*　Java     
*　JavaScript     
*　TypeScript.     
*　Python    
*　Ruby     
*　PHP     
*　C#     
*　Go     
*　Swift     
*　Kotlin     
*　Rust     
*　Scala     
*　Dart     
*　Objective-C     
*　PowerShell     
*　Shell     



# how to use 

## Quick Start
Java, Python and Javascript are among the most popular languages for TikTok OpenAPI third-party developers. The TikTok OpenAPI  SDK is a code package that provides an interface between your application and Tiktok's  OpenAPI for these three languages. This tutorial provides the basic knowledge needed to access to our SDK and includes sample code for your reference.

## Version

- API version: 0.1.0 Beta (in Beta Test)

## Prerequisites
  1.   [Register your app](https://developers.tiktok.com/doc/getting-started-create-an-app)
  4.   [Manage User Access Tokens with OAuth v2](https://developers.tiktok.com/doc/login-kit-manage-user-access-tokens)
  5.   [Obtain authentication](https://ads.tiktok.com/marketing_api/docs?id=1738373164380162)

## Integration with different lanauage SDK


### Integration with Java SDK

#### Version requirements

  - Java 1.7+
  - Maven 
  
#### Integration Steps:
1. Download the TikTok Business API SDK
   ```
    git clone https://github.com/tiktok/tiktok-business-api-sdk.git
   ```
2. Set up your local environment
   - Add this dependency to your project's POM if you are using Maven, otherwise you could skip this step.
   ```
    <dependency>
      <groupId>io.swagger</groupId>
      <artifactId>business_api_client</artifactId>
      <version>1.0.0</version>
      <scope>compile</scope>
    </dependency>
   ```
   - Build the SDK jar file with following the commands
   ```
    cd your_path/tiktok_business_api_sdk/java_sdk
    mvn install package
   ```
   - Manually import the JAR files

     **Note:** If you are using IntelliJ, within your project, click on File-> Project Structure -> Modules and click the '+' to add the jar you built in the previous step.

   - Install the SDK package within your project
   ```
    cd your project
    mvn install package
   ```
3. Run and test the SDK
   - Create a class and import it to the location where you're able to run it. Below are the sample codes for your reference.

   ```
    import io.swagger.client.api.ToolApi;
    import java.util.*;
    import static org.junit.Assert.*;
    import business_api.Response;


    public class Tool {

        ToolApi apiInstance;
        String accessToken = "Your_access_token";
        String advertiserId = "Your_advertiser_id";
        public Tool() {
            ToolApi instance = new ToolApi();
            this.apiInstance = instance;
        }
        public void ToolLanguageTest() {
            try {
                Response result = apiInstance.toolLanguage(advertiserId, accessToken);
                assertNotNull(result);
                System.out.println(result);
                System.out.println("Success ToolApi#toolLanguage");

            } catch (Exception e) {
                System.err.println("Exception when calling ToolApi#toolLanguage");
                e.printStackTrace();
            }
        }
     }
    ```
## Integration with Python SDK

#### Version requirements

  - Python 3.4+

#### Integration Steps:
1. Download the TikTok Business API SDK
   ```
    git clone https://github.com/tiktok/tiktok-business-api-sdk.git
   ```
2. Set up the Python virtual environment by using the following command. If you already have your own virtual environment, please skip this step and source into your own project.
   ```
    python3 -m venv your_virtual_env
    source your_virtual_env/bin/activate
   ```
3. Set Local env and install dependency with following commands
   ```
    export PYTHONPATH=tiktok_business_api_sdk/python_sdk:tiktok_business_api_sdk/python_sdk/business_api_client
    cd your_path/tiktok_business_api_sdk
    pip install -r requirements.txt
   ```
4. Run and test the Python SDK. Below are the sample codes for your reference.
   ```
    from __future__ import print_function
    from Your_path.tiktok_business_api.python_sdk import business_api_client
    from pprint import pprint

    TEST_ADVERTISER_ID = 'Your_advertiser_id'
    TEST_ACCESS_TOKEN = 'Your_access_token'


    def test_tool_language():
    # create an instance of the API class
        api_instance = business_api_client.ToolApi()
        advertiser_id = TEST_ADVERTISER_ID # str |
        access_token = TEST_ACCESS_TOKEN # str |

        try:
            api_response = api_instance.tool_language(advertiser_id, access_token)
            pprint(api_response)
        except ApiException as e:
            print("Exception when calling ToolApi->tool_language: %s\n" % e)
   ```
  
## Integration with JavaScript SDK

#### Version requirements

  - Javascript v13+, up to v18
  - npm
 
#### Integration Steps:
1. Download the TikTok Business API SDK
   ```
    git clone https://github.com/tiktok/tiktok-business-api-sdk.git
   ```
2. Set Local env and install the dependencies
   - Install js-sdk dependencies
   
   ```
    cd your_path/tiktok_business_api_sdk/js_sdk
    npm install
   ```
   - In your project, run the following command 
   
   ```
    npm install your_path/tiktok_business_api_sdk/js_sdk --save
   ```
   - In your project, add the following code in package.json to set js_sdk as module
   ```
    {
     ...
       "type": "module",
     ...
    }
   ```
3. Run and test JavaScript SDK
   - Import the SDK to your JavaScript file and add the methods you would like to use. Below are the sample codes for your reference. 
   
   ```
    import * as js_sdk from "business_api_client"

    let api = new js_sdk.ToolApi()
    let advertiserId = "Your_advertiser_id"; 
    let accessToken = "Your_accessToken_id"; 
    api.toolLanguage(advertiserId, accessToken, (error, data, response) => {
      if (error) {
        console.error(error);
      } else {
        console.log('toolLanguage called successfully. Returned data: ' + JSON.stringify(data));
      }
    });
   ```
   
## Give feedback

- If you want to report bugs or issues, please visit [TikTok API for Business Developer Portal](https://ads.tiktok.com/marketing_api/homepage) and click "?"  on the top 
right to submit a ticket under Marketing API category.

## References

Here are the detailed documentation for currently supported programming languages.

- Java:  please refer to [java_sdk/README.md](https://github.com/tiktok/tiktok-business-api-sdk/blob/main/java_sdk/README.md)
- Python:  please refer to [python_sdk/README.md](https://github.com/tiktok/tiktok-business-api-sdk/blob/main/python_sdk/README.md)
- JavaScript:  please refer [js_sdk/README.md](https://github.com/tiktok/tiktok-business-api-sdk/blob/main/js_sdk/README.md)

