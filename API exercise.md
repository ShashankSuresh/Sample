# API exercise

This exercise tests a candidate's ability to troubleshoot issues around requests to [Front's Core API](https://dev.frontapp.com/docs/core-api-overview).  Use the linked documentation as reference to explain why the requests failed and suggest a solution.

## Question #1:

#### Request - [docs 🔗](https://dev.frontapp.com/docs/fundamentals)
```
curl --request GET \
  --url https://api2.frontapp.com/conversations/cnv_123/drafts
```
#### Response
```
{
    "_error": {
        "status": 401,
        "title": "Unauthenticated",
        "message": "JSON Web Token error"
    }
}
```
### Answer #1: 


The Authentication documentation states that The Core API uses API tokens to authenticate its user.
The HTTP authentication scheme is carried out by the Bearer authentication. Hence the Authorization header must include the JWT token for each and every request under the below format:

Curl example: ``` --header 'Authorization: Bearer [REQUEST_TOKEN]' ```

The example above is a GET request without the Bearer token in the Authentication Header. Hence the request fails with an error “JSON Web Token error”.
In order to solve this, one can create a valid API token from Front's API settings. (Please refer to the screengrab below)

Ref: https://help.front.com/t/36dsxd/how-to-create-and-revoke-api-tokens


![alt text](JWTToken.JPG)

---

## Question #2:

#### Request - [docs 🔗](https://dev.frontapp.com/docs/rate-limiting)
```
curl --request POST \
     --url https://api2.frontapp.com/channels/cha_123/messages \
     --header 'Accept: application/json' \
     --header 'Authorization: Bearer 08yppb3qDjpnbUbiOrZ4cA2d1EECFm' \
     --header 'Content-Type: application/json' \
     --data '
{
     "to": [
          "support@frontapp.com"
     ],
     "options": {
          "archive": true
     },
     "body": "test"
}
'
```
#### Response
```
{
    "_error": {
        "status": 429,
        "title": "Too many requests",
        "message": "Rate limit exceeded for tier-2 route. Please retry in 341 milliseconds."
    }
}
```
### Answer #2: 

Front’s API RPM limit depends on factors such as Customer Relationship plans (Starter, Prime, Enterprise), authentication type (OAuth) and the integration as well. 

In this example, the reason why we are thrown with an error ‘status: 429 and message: Rate limit exceeded for tier-2 route. Please retry in 341 milliseconds.’ Is because the POST method of tire-2 which is resource intensive routes are limited to 5 requests per resources per second for an endpoint. When we exceed the given rate limits, we receive 429 status code X-Front-Tier followed by header Retry-After how many seconds you need to wait before you can retry the request.

In order to avoid the error, we should check for the failures and make sure we increase the API rate limit beyond the limit provided for the respective plan. To make necessary modifications to the API’s rate limit one should contact their customer success manager or account manager or Front’s support team at support@frontapp.com.

Please refer to the below Postman response screengrab where the rate limit parameters are seen.  

![alt text](ScreenshotQ2.JPG)

---
## Question #3:

#### Request - [docs 🔗](https://dev.frontapp.com/reference/post_conversations-conversation-id-messages)
```
curl --request POST \
     --url https://api2.frontapp.com/conversations/cnv_123/messages \
     --header 'Accept: application/json' \
     --header 'Authorization: Bearer 08yppb3qDjpnbUbiOrZ4cA2d1EECFm' \
     --header 'Content-Type: application/json' \
     --data '
{
     "options": {
          "archive": false
     },
     "body": "Answer from the API",
     "author_id": alt:email:support@frontapp.com
}
'
```
#### Response
```
{
    "_error": {
        "message": "Unexpected token a in JSON at position 109"
    }
}
```
### Answer #3: 

For the given POST request we could see that the ```author_id``` is of type ```string``` and this has not been enclosed within commas due to which we received an error "message": "Unexpected token a in JSON at position 109". Concentrating on the data type could avoid these kinds of issues.

![alt text](AuthorIDType.JPG)

![alt text](Q3-Screenshot2.JPG)

---
## Question #4:

#### Request - [docs 🔗](https://dev.frontapp.com/reference/post_inboxes)
```
curl --request POST \
     --url https://api2.frontapp.com/inboxes \
     --header 'Authorization: Bearer 08yppb3qDjpnbUbiOrZ4cA2d1EECFm' \
     --header 'Content-Type: application/json' \
     --data '
{
     "teammate_ids": [
          "tea_123",
          "tea_abc",
          "tea_def"
     ]
}
'
```
#### Response
```
{
    "_error": {
        "status": 400,
        "title": "Bad request",
        "message": "Body did not satisfy requirements"
    }
}
```
### Answer #4: 

POST requests are used to send data to the API server to create or update the resource. The data sent to the server is stored in the request body of the HTTP request. As per the documentation, for this request the ```name``` parameter is mandatory of type ```string```. We see that ```name``` parameter is not added in the body of the request due to which we received the 400 bad request error. While dealing with POST method we need to validate the parameters passed to the API request.

![alt text](NameParams.JPG)

![alt text](Q4-Screenshot1.JPG)

---
## Question #5:

#### Request - [docs 🔗](https://dev.frontapp.com/reference/patch_conversations-conversation-id)
```
curl --request PATCH \
     --url https://api2.frontapp.com/conversations/cnv_123 \
     --header 'Authorization: Bearer 08yppb3qDjpnbUbiOrZ4cA2d1EECFm' \
     --header 'Content-Type: application/json' \
     --data '
{
  "tag_ids":"test"
}
'
```
#### Response
```
  "_error": {
      "status": 400,
      "title": "Bad request",
      "message": "Body did not satisfy requirements"
  }
```
### Answer #5: 

PATCH method is used for making partial changes to an existing resource. In the above request the ```tag_ids``` parameter is a ```string``` but as per the documentation it's supposed to be an ```array of strings```. Updating this will result in a 200 OK response. 

![alt text](TagArray.JPG)

![alt text](ScreenshotQ5.JPG)

---
## Question #6:

#### Request - [docs 🔗](https://dev.frontapp.com/reference/post_conversations-conversation-id-messages)
```
curl --request POST \
     --url https://api2.frontapp.com/conversations/cn_123/messages \
     --header 'Accept: application/json' \
     --header 'Authorization: Bearer 08yppb3qDjpnbUbiOrZ4cA2d1EECFm' \
     --header 'Content-Type: application/json' \
     --data '
{
     "options": {
          "archive": true
     },
     "body": "Test"
}
'
```
#### Response
```
{
    "_error": {
        "status": 404,
        "title": "Not found",
        "message": "Unknown conversation ID cn_123"
    }
}
```
### Answer #6: 

The reason why the above API Post request fails with the "message": "Unknown conversation ID cn_123" is because, the type of a conversation ID is ```cnv_:number``` but we are passing it as ```cn_123```. 

By introducting schema validation libraries such as Yup or Joi will help capture the invalid schema even before firing an API call.

![alt text](CNVFormat.JPG)

![alt text](Q6Screenshot1.JPG)

---
