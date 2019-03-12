#Serverless

## Lambda

### Layers of Extraction
* Data Centres
* Hardware
* Assembly Code/Protocols
* High level languages
* Operating Systems
* Application Layer/AWS APIs
* AWS Lambda

### What is it?
_A compute service where you can upload your code and create Lambda functions, and takes care of provisioning and managing the servers that you use to run the code_

Can be used in the following ways:
  * Event-driven compute service - AWS Lambda runs your code in response to events
  * Compute service to run code in response to HTTP requests using Amazon API Gateway or API calls made using AWS SDKs

### Which languages are supported?
* Node.js
* Java
* Python
* C#
* Go

### Pricing
* Num of requests: First 1 million requests free, $0.20/million after
* Duration: calculated from time code begins executing until it returns. Price depends on amount of memory allocated to your function. $0.00001667 for every GB-second used

### Positives
* No servers
* Scales out (not up) automatically
  * Can have functions running in parallel at the same time
* Inexpensive

### Exam Tips
* Use AWS X-Ray to debug Lambda
* Other services that are Serverless: DynamoDB, S3, API Gateway
* RDS and EC2 are **not** Serverless
* Can use it across regions

## API Gateway

### Types of APIs
* **REST APIs** (Representational State Transfer)
  * Uses JSON
* **SOAP APIs** (Simple Object Access Protocol)
  * Uses XML

_API Gateway runs on RESTful APIs.  It is a fully managed service that makes it easy for developers to publish, maintain, monitor, and secure APIs at any scale._

### What can API Gateway do?
* Expose HTTPS endpoints to define a RESTful API (gives HTTPS address that we can make calls to)
* Serverless-ly connect to services like Lambda & DynamoDB
* Send each API endpoint to a different target
* Efficient and inexpensive
* Scales automatically
* Track and control usage by API key
* Throttle requests to prevent attacks
* Connect to CloudWatch to log all requests for monitoring
* Maintain multiple versions of your API

### How do I configure API Gateway?
* Define an API (container)
* Define resources and nested resources (URL paths)
* For each resource...
  * Select supported HTTP methods (GET, DELETE etc)
  * Set security
  * Choose target
  * Set request and response transformations
* Deploy API to a stage
  * Uses API Gateway domain by default but can use custom domain

### API Caching
Can enable API caching in API Gateway to cache your endpoint's response. This can reduce the number of calls made to your endpoint and improve latency of the requests to your API

When caching enabled for a stage, API Gateway caches responses from your endpoint for a specified time-to-live period in seconds.

Gateway then responds to the request by looking up the endpoint response from the cache instead of making a request to your endpoint

### Same Origin Policy
Under the policy, a web browser permits scripts contained in a first web page to page to access data in a second web page, but only if both have the same origin. This is done to prevent Cross-Site Scripting (XSS) attacks

_This is enforced by web browsers but is ignored by tools like PostMan and curl_

### CORS (Cross Origin Resource Sharing)
A mechanism that allows restricted resources on a web page to be requested from another domain outside the domain from which the first resource was served. Enforced by the client

_This is a way for API Gateway to talk to S3_

* Browser makes an HTTP OPTIONS call (GET, POST etc) for a URL
* Server returns a response
* Example of an error can be: _"Origin policy cannot be read at the remote resoure"._ You need to enable CORS on API Gateway
  * If you are using Javascript/AJAX that uses multiple domains with API Gateway, need to enable

## Lab
1. Create S3 bucket
  - Go to properties in S3 and enable static web hosting
  - Type in index document and error document
2. Go to Route53 and register domain (needs to be same name as S3 bucket)
3. Configure Lambda function
  - Can create from 3 different ways (from scratch, from preconfigured templates, or from repos built by other developers)
  - Fill in fields: name, runtime, role, policy template (use simple microservice permissions)
  - Write function in Lambda window
  - Configure triggers (for exam, memorize triggers!): add API Gateway -> create new API
4. Configure API Gateway
  - Create action method (integration type: Lambda function)
5. Deploy API
  - Can click on 'invoke API' to get response
6. Use API Gateway URL in your code
7. Go to S3 and add files to your bucket (index.html, error.html), go to Route53 and create record set and set alias to Yes

## Version Control with Lambda

### Versioning
* When using versioning, can publish one or more versions of your Lambda function
* Each Lambda function version has a unique ARN (Amazon Resource Name)
* After you publish a version, it cannot be changed
* Latest code is **$LATEST** version

### Qualified/Unqualified ARNs
* **Qualified ARN:** The function ARN with the version suffix, uses $latest
* **Unqualified ARN:** The function ARN without the version suffix

## Alias
Name that maps to a specific version of your function
