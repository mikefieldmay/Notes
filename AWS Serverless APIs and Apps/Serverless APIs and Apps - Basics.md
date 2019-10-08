# Serverless APPS and Apis

## What is AWS

AWS Amazon Web Services offers a broad range of cloud computing services. We rent their infrastructure to run our solutions and applications. With these services you can do basically anything web related.
AWS has a massive infrastructure setup. They have different regions that each contain hosting platforms.
Solutions are a term that try to make starting on AWS easier. 
- EC2 instances are servers that you can start up and use (e.g to host a website or an api). 
- Elastic Beanstalk is a wrapper around other services that enable us toquickly and easily spin up new services.

## IAM Account Security

Identity and Access management is a service that handles your security. 

## What is Serverless Computing

Traditionally we create apps with a backend ( a restful api that holds out data). Another more traditional approach is to have a server returning html for each request but this is not the best approach for serverless programming. It works best with backend that is decoupled from the frontend. There are limitiations with this approach. We need to recreate the infrastructure everytime we want to create a backend. We also have to physically run the servers. They are online even if not required, keep the software updated and also provision them when things are busy.
With Serverless apps we can create a special function that only runs when we are actually accessing the api. We are also able to host our website in a serverless manner too. It scales automatically aswell.

## Core AWS Services

When using the serverless approach, you can do much more than just hosting a backend. You can even host your app and it will scale automatically.
- Serve Static Web App => Amazon S3 (Simple Storage Service). Basic file storage.
- REST API => API Gateway
- AWS Lambda => Execute code on demand
- Database => Dynamo DB (no sql and we don't have to assign capacity)
- Authentication => AWS Cognito. Easily create user pools with specified access to user services
- DNS => Translate URL with Route 53
- Improve Performance Caching => Cloudfront

## API Gateway

With API gateway we can create restful APIs. Our web app/mobile app will send requests to these endpoints. API Gateway lets us create endpoints with methods and also handle who can access these endpoints. We are able to use this to then access other services, or triggerlambda code. API keys are important when creating APIs on the gateway if you plan on exposing your API to other developers.
A resource is just a new path. In order for our changes to be reflected on live, we need to deploy an api to a stage. 
A snapshot of your api is taken and is shipped to the web. You can view these live apis in the `Stages` tab. You cannot edit the api here, but you can view some details about the deployment.

### Request response cycle

API Gateway shows us how we interact with the gateway. We have a Client test option which we can use to test the api.
- Method Request we can use to configure who has access to the service. We can block incoming requests that aren't authorised or we can validate wheter our query params, headers or body are correct. It ensures that our requests are exactly the way we want them to be.
- Integration Request is about mapping the data for how we want to use it in the next step of the cycle. It triggers the next endpoint in the cycle aswell. 
- This cpart could be a mock response, a new endpoint or a lambda functoin
- Integration Response then allows us to extract data from the response we're sending back. We can add any headers we want to set as well as mapping the data to the structure that we want to return.
- Method response allows us to configure the shape of the response it's sending back e.g using headers and models.

### Requests
When we create a request, when we want to access data on an api that comes from a different domain it is prevented by default. The server needs to send headers that tell the client this cross domain access is okay. When using modern browsers typically pre-flight requests are sent to an api. We need to handle this OPTIONS request so we can let the browser know its still okay to use this server. Access-Control-Allow-Headers are the typical headers we return to let the browser know we can access from a different domain.

### AWS Lambda

AWS lambda is a service that lets you host code on it and run it on certain events. There are different event sources that can trigger lambda functions. Examples are S3 (e.g when a file gets uploaded), Cloudwatch (scheduled), API Gateway (HTTP Request).
The code triggered can be NodeJS, Python, Java or c#. It can interact with other aws services and then return a response through a callback. We will likely have a different lambda function for each endpoint.