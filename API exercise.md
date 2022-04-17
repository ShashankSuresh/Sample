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

`The request lacks user authenticationas as part of validation.Using the above URL GET method will try to retrieve information from the server to which we must include URL with the JWT token in the authorisation header.
Here, we are not sending the token for the above request in the Authorization header due to which we are thrown with the mentioned error.
for the rectification of the above issue, Created a token using the Front API token and vaslidated the URL and bearer token in the postman.`

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

`Customer depending upon the plan have different rate limits for the messages. Here, for the POST method of tire-2 which is resource intensive routes are limited to 5 requests per resources per second for an endpoint. When we exceed the given rate limits we recieve 429 status code X-Front-Tier followed by header Retry-After how many seconds you need to wait before you can retry the request.
In order to avoid the error we should,
check for the failures and make sure we increase the API rate limit beyond the limit provided by respected plan, have a word with customer success or account manager, or email support@frontapp.com.`

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

For the given POST request we could see that the Author_id is STRING type and we have not enclosed it with the double inverted commas due to which we received an error "message": "Unexpected token a in JSON at position 109". Concentrating on the data rtype could avoid these kinds of issues.

![alt text](Q3-Screenshot1.JPG)
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

POST requests are used to send data to the API server to create or update the resource. The data sent to the server is stored in the request body of the HTTP request. Here, we could see that the body is not defined properly. Body paramters has NAME as a String and TEAMMATES_IDS as array of String and we see that "Name" String is not added in the body of the request due to which we received the 400 bad request error. While dealing with POST method we need to validate the parameters passed to the Server.

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

PATCH method is used for making the partial changes to an existing resources.Here, We are trying to modify the Conversations wherein from the mentioned body parameters Tag_ids is the "Array of String" but we are passing one string due to which we received 400 bad request.
By reiterating the requirements passed in the bosy parameters we can avoid these issues. 

![alt text](ScreenshotQ2.JPG)

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

In the Path Parameters conversation ID string is defined as CNV_123 but we are passing CN_123 in the URL due to which we received error message as "Unknown conversation ID cn_123".

![alt text](Q6Screenshot1.JPG)

---
