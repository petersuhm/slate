---
title: Timekit API v2

toc_footers:
- <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Timekit API v2

> ### Produces
> **Schemes**: `http`  
> **Host**: `petstore.swagger.io`  
> **Base path**: `/v2`  

### Authorization: petstore_auth
Type | Description
--- | ---
oauth2 | 
### Authorization: api_key
Type | Name | In | Description
--- | --- | --- | ---
apiKey | api_key | header | 

# Building booking experiences with Timekit

### INTRODUCTION

Timekit's booking engine is a hosted and flexible solution to handle booking flows for your app. Bookings can follow different graphs, which are a pre-defined set of steps that can trigger actions upon state change, such as sending out emails or trigger webhooks. The booking engine has graphs for many different use cases, such as instant bookings, bookings that need confirmation, bookings that involve a payment step, bookings that involve groups or classes etc.

### UNDERSTANDING GRAPHS

Graphs are the "blueprints" that bookings are based on. They define different actions and transitions that bookings can go through, based on your business logic. If you are looking to set up a 1-to-1 booking flow, currently there are two graphs available:

1) instant - Instant booking where new bookings are auto-confirmed
2) confirm_decline - New bookings start in a tentative state and you can either confirm or decline them.

If you are looking to set up a 1-to-many, or group, booking flow, you should read our guide on groups and classes.

When you create a booking, you chose which flow graph it should follow. A JSON representation of the flow graphs can be retrieved by calling this endpoint and a Graphviz PNG representation of the flow by calling this instead.

<img src="https://files.readme.io/f78347e-confirm_decline_graph.png">

# Authentication

All authentication is user-based and working with the API is based on the current authenticated user's privileges. Each user only has access to their own calendars, events, meetings etc. with a few exceptions (/findtime, /events/availability, /meetings/:id)

### BASIC AUTHENTICATION
For simple email/password authentication, we use Basic Authentication over HTTPS. Auth is stateless and is sent as Base64 encoded header with each HTTP request.

Each user have a password which is used to retrieve an API token that should go with every subsequent request. You retrieve your a user's API token by calling:

```http
POST https://api.timekit.io/v2/auth HTTP/1.1
Content-Type: application/json

{
    "email": "doc.brown@timekit.io",
    "password": "DeLorean"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": {
        "activated": true,
        "email": "doc.brown@timekit.io",
        "first_name": "Dr. Emmett",
        "img": "http://www.gravatar.com/avatar/7a613e5348d63476276935025",
        "last_name": "Brown",
        "last_sync": null,
        "name": "Dr. Emmett Brown",
        "timezone": "America/Los_Angeles",
        "token": "UZpl3v3PTP1PRwqIrU0DSVpbJkNKl5gN",
        "token_generated_at": null,
        "api_token": "FluxCapacitator"
    }
}
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
email<span title="required" class="required">&nbsp;*&nbsp;</span> | body | string | The email of the user
password<span title="required" class="required">&nbsp;*&nbsp;</span> | body | string | The password of the user

### Responses
Http code | Type | Description
--- | --- | ---
200 | OK | User was successfully authenticated
401 | Unauthorized | Could not authorize user based on credentials

# Booking
## Add a new pet to the store

```http
POST /v2/pet HTTP/1.1
Content-Type: application/json

{
    "body": {
        "id": "integer",
        "category": {
            "id": "integer",
            "name": "string"
        },
        "name": "string",
        "photoUrls": [
            "string"
        ],
        "tags": [
            {
                "id": "integer",
                "name": "string"
            }
        ],
        "status": "string"
    }
}
```
	
```http
HTTP/1.1 405 Method Not Allowed
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
body<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [Pet](#pet) | Pet object that needs to be added to the store

### Responses
Http code | Type | Description
--- | --- | ---
405 | no content | Invalid input

## Update an existing pet

```http
PUT /v2/pet HTTP/1.1
Content-Type: application/json

{
    "body": {
        "id": "integer",
        "category": {
            "id": "integer",
            "name": "string"
        },
        "name": "string",
        "photoUrls": [
            "string"
        ],
        "tags": [
            {
                "id": "integer",
                "name": "string"
            }
        ],
        "status": "string"
    }
}
```
	
```http
HTTP/1.1 400 Bad Request
```
```http
HTTP/1.1 404 Not Found
```
```http
HTTP/1.1 405 Method Not Allowed
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
body<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [Pet](#pet) | Pet object that needs to be added to the store

### Responses
Http code | Type | Description
--- | --- | ---
400 | no content | Invalid ID supplied
404 | no content | Pet not found
405 | no content | Validation exception


## Finds Pets by status

```http
GET /v2/pet/findByStatus HTTP/1.1
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "id": "integer",
        "category": {
            "id": "integer",
            "name": "string"
        },
        "name": "string",
        "photoUrls": [
            "string"
        ],
        "tags": [
            {
                "id": "integer",
                "name": "string"
            }
        ],
        "status": "string"
    }
]
```
```http
HTTP/1.1 400 Bad Request
```

Multiple status values can be provided with comma separated strings


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
status<span title="required" class="required">&nbsp;*&nbsp;</span> | query | array[string] | Status values that need to be considered for filter

### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[Pet](#pet)] | successful operation
400 | no content | Invalid status value


## Finds Pets by tags

```http
GET /v2/pet/findByTags HTTP/1.1
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "id": "integer",
        "category": {
            "id": "integer",
            "name": "string"
        },
        "name": "string",
        "photoUrls": [
            "string"
        ],
        "tags": [
            {
                "id": "integer",
                "name": "string"
            }
        ],
        "status": "string"
    }
]
```
```http
HTTP/1.1 400 Bad Request
```

Muliple tags can be provided with comma separated strings. Use tag1, tag2, tag3 for testing.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
tags<span title="required" class="required">&nbsp;*&nbsp;</span> | query | array[string] | Tags to filter by

### Responses
Http code | Type | Description
--- | --- | ---
200 | array[[Pet](#pet)] | successful operation
400 | no content | Invalid tag value


## Find pet by ID

```http
GET /v2/pet/{petId} HTTP/1.1
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "integer",
    "category": {
        "id": "integer",
        "name": "string"
    },
    "name": "string",
    "photoUrls": [
        "string"
    ],
    "tags": [
        {
            "id": "integer",
            "name": "string"
        }
    ],
    "status": "string"
}
```
```http
HTTP/1.1 400 Bad Request
```
```http
HTTP/1.1 404 Not Found
```

Returns a single pet


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
petId<span title="required" class="required">&nbsp;*&nbsp;</span> | path | integer | ID of pet to return

### Responses
Http code | Type | Description
--- | --- | ---
200 | [Pet](#pet) | successful operation
400 | no content | Invalid ID supplied
404 | no content | Pet not found

## Updates a pet in the store with form data

```http
POST /v2/pet/{petId} HTTP/1.1
```
	
```http
HTTP/1.1 405 Method Not Allowed
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
petId<span title="required" class="required">&nbsp;*&nbsp;</span> | path | integer | ID of pet that needs to be updated
name | formData | string | Optional. Updated name of the pet
status | formData | string | Optional. Updated status of the pet

### Responses
Http code | Type | Description
--- | --- | ---
405 | no content | Invalid input

## Deletes a pet

```http
DELETE /v2/pet/{petId} HTTP/1.1
api_key: string
```
	
```http
HTTP/1.1 400 Bad Request
```
```http
HTTP/1.1 404 Not Found
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
api_key | header | string | Optional. 
petId<span title="required" class="required">&nbsp;*&nbsp;</span> | path | integer | Pet id to delete

### Responses
Http code | Type | Description
--- | --- | ---
400 | no content | Invalid ID supplied
404 | no content | Pet not found


## uploads an image

```http
POST /v2/pet/{petId}/uploadImage HTTP/1.1
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": "integer",
    "type": "string",
    "message": "string"
}
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
petId<span title="required" class="required">&nbsp;*&nbsp;</span> | path | integer | ID of pet to update
additionalMetadata | formData | string | Optional. Additional data to pass to server
file | formData | file | Optional. file to upload

### Responses
Http code | Type | Description
--- | --- | ---
200 | [ApiResponse](#apiresponse) | successful operation


# Store
## Returns pet inventories by status

```http
GET /v2/store/inventory HTTP/1.1
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

"object"
```

Returns a map of status codes to quantities


### Responses
Http code | Type | Description
--- | --- | ---
200 | object | successful operation


## Place an order for a pet

```http
POST /v2/store/order HTTP/1.1
Content-Type: application/json

{
    "body": {
        "id": "integer",
        "petId": "integer",
        "quantity": "integer",
        "shipDate": "string",
        "status": "string",
        "complete": "boolean"
    }
}
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "integer",
    "petId": "integer",
    "quantity": "integer",
    "shipDate": "string",
    "status": "string",
    "complete": "boolean"
}
```
```http
HTTP/1.1 400 Bad Request
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
body<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [Order](#order) | order placed for purchasing the pet

### Responses
Http code | Type | Description
--- | --- | ---
200 | [Order](#order) | successful operation
400 | no content | Invalid Order


## Find purchase order by ID

```http
GET /v2/store/order/{orderId} HTTP/1.1
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "integer",
    "petId": "integer",
    "quantity": "integer",
    "shipDate": "string",
    "status": "string",
    "complete": "boolean"
}
```
```http
HTTP/1.1 400 Bad Request
```
```http
HTTP/1.1 404 Not Found
```

For valid response try integer IDs with value >= 1 and <= 10. Other values will generated exceptions


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
orderId<span title="required" class="required">&nbsp;*&nbsp;</span> | path | integer | ID of pet that needs to be fetched

### Responses
Http code | Type | Description
--- | --- | ---
200 | [Order](#order) | successful operation
400 | no content | Invalid ID supplied
404 | no content | Order not found

## Delete purchase order by ID

```http
DELETE /v2/store/order/{orderId} HTTP/1.1
```
	
```http
HTTP/1.1 400 Bad Request
```
```http
HTTP/1.1 404 Not Found
```

For valid response try integer IDs with positive integer value. Negative or non-integer values will generate API errors


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
orderId<span title="required" class="required">&nbsp;*&nbsp;</span> | path | integer | ID of the order that needs to be deleted

### Responses
Http code | Type | Description
--- | --- | ---
400 | no content | Invalid ID supplied
404 | no content | Order not found


# User
## Create user

```http
POST /v2/user HTTP/1.1
Content-Type: application/json

{
    "body": {
        "id": "integer",
        "username": "string",
        "firstName": "string",
        "lastName": "string",
        "email": "string",
        "password": "string",
        "phone": "string",
        "userStatus": "integer"
    }
}
```
	
```http
HTTP/1.1 [default] 
```

This can only be done by the logged in user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
body<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [User](#user) | Created user object

### Responses
Http code | Type | Description
--- | --- | ---
default | no content | successful operation


## Creates list of users with given input array

```http
POST /v2/user/createWithArray HTTP/1.1
Content-Type: application/json

{
    "body": [
        {
            "id": "integer",
            "username": "string",
            "firstName": "string",
            "lastName": "string",
            "email": "string",
            "password": "string",
            "phone": "string",
            "userStatus": "integer"
        }
    ]
}
```
	
```http
HTTP/1.1 [default] 
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
body<span title="required" class="required">&nbsp;*&nbsp;</span> | body | array[[User](#user)] | List of user object

### Responses
Http code | Type | Description
--- | --- | ---
default | no content | successful operation


## Creates list of users with given input array

```http
POST /v2/user/createWithList HTTP/1.1
Content-Type: application/json

{
    "body": [
        {
            "id": "integer",
            "username": "string",
            "firstName": "string",
            "lastName": "string",
            "email": "string",
            "password": "string",
            "phone": "string",
            "userStatus": "integer"
        }
    ]
}
```
	
```http
HTTP/1.1 [default] 
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
body<span title="required" class="required">&nbsp;*&nbsp;</span> | body | array[[User](#user)] | List of user object

### Responses
Http code | Type | Description
--- | --- | ---
default | no content | successful operation


## Logs user into the system

```http
GET /v2/user/login HTTP/1.1
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

"string"
```
```http
HTTP/1.1 400 Bad Request
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
username<span title="required" class="required">&nbsp;*&nbsp;</span> | query | string | The user name for login
password<span title="required" class="required">&nbsp;*&nbsp;</span> | query | string | The password for login in clear text

### Responses
Http code | Type | Description
--- | --- | ---
200 | string | successful operation
400 | no content | Invalid username/password supplied


## Logs out current logged in user session

```http
GET /v2/user/logout HTTP/1.1
```
	
```http
HTTP/1.1 [default] 
```

### Responses
Http code | Type | Description
--- | --- | ---
default | no content | successful operation


## Get user by user name

```http
GET /v2/user/{username} HTTP/1.1
```
	
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": "integer",
    "username": "string",
    "firstName": "string",
    "lastName": "string",
    "email": "string",
    "password": "string",
    "phone": "string",
    "userStatus": "integer"
}
```
```http
HTTP/1.1 400 Bad Request
```
```http
HTTP/1.1 404 Not Found
```

### Parameters
Name | In | Type | Description
--- | --- | --- | ---
username<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | The name that needs to be fetched. Use user1 for testing. 

### Responses
Http code | Type | Description
--- | --- | ---
200 | [User](#user) | successful operation
400 | no content | Invalid username supplied
404 | no content | User not found

## Updated user

```http
PUT /v2/user/{username} HTTP/1.1
Content-Type: application/json

{
    "body": {
        "id": "integer",
        "username": "string",
        "firstName": "string",
        "lastName": "string",
        "email": "string",
        "password": "string",
        "phone": "string",
        "userStatus": "integer"
    }
}
```
	
```http
HTTP/1.1 400 Bad Request
```
```http
HTTP/1.1 404 Not Found
```

This can only be done by the logged in user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
username<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | name that need to be updated
body<span title="required" class="required">&nbsp;*&nbsp;</span> | body | [User](#user) | Updated user object

### Responses
Http code | Type | Description
--- | --- | ---
400 | no content | Invalid user supplied
404 | no content | User not found

## Delete user

```http
DELETE /v2/user/{username} HTTP/1.1
```
	
```http
HTTP/1.1 400 Bad Request
```
```http
HTTP/1.1 404 Not Found
```

This can only be done by the logged in user.


### Parameters
Name | In | Type | Description
--- | --- | --- | ---
username<span title="required" class="required">&nbsp;*&nbsp;</span> | path | string | The name that needs to be deleted

### Responses
Http code | Type | Description
--- | --- | ---
400 | no content | Invalid username supplied
404 | no content | User not found



# Models
## Order
```json
{
    "id": "integer",
    "petId": "integer",
    "quantity": "integer",
    "shipDate": "string",
    "status": "string",
    "complete": "boolean"
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id | integer | int64 | 
petId | integer | int64 | 
quantity | integer | int32 | 
shipDate | string | date-time | 
status | string | <div>Acceptable values:</div><ul class="enum"><li>placed</li><li>approved</li><li>delivered</li></ul> | Order Status
complete | boolean |  | 

	
## Category
```json
{
    "id": "integer",
    "name": "string"
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id | integer | int64 | 
name | string |  | 

	
## User
```json
{
    "id": "integer",
    "username": "string",
    "firstName": "string",
    "lastName": "string",
    "email": "string",
    "password": "string",
    "phone": "string",
    "userStatus": "integer"
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id | integer | int64 | 
username | string |  | 
firstName | string |  | 
lastName | string |  | 
email | string |  | 
password | string |  | 
phone | string |  | 
userStatus | integer | int32 | User Status

	
## Tag
```json
{
    "id": "integer",
    "name": "string"
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id | integer | int64 | 
name | string |  | 

	
## Pet
```json
{
    "id": "integer",
    "category": {
        "id": "integer",
        "name": "string"
    },
    "name": "string",
    "photoUrls": [
        "string"
    ],
    "tags": [
        {
            "id": "integer",
            "name": "string"
        }
    ],
    "status": "string"
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
id | integer | int64 | 
category | [Category](#category) |  | 
name<span title="required" class="required">&nbsp;*&nbsp;</span> | string |  | 
photoUrls<span title="required" class="required">&nbsp;*&nbsp;</span> | array[string] |  | 
tags | array[[Tag](#tag)] |  | 
status | string | <div>Acceptable values:</div><ul class="enum"><li>available</li><li>pending</li><li>sold</li></ul> | pet status in the store

	
## ApiResponse
```json
{
    "code": "integer",
    "type": "string",
    "message": "string"
}
```
	
### Fields
Name | Type | Format | Description
--- | --- | --- | ---
code | integer | int32 | 
type | string |  | 
message | string |  | 

	


<style>
    ul.enum {
        margin: 0;
        padding: 0 0 0 2px;
        list-style-position: inside;
    }
    .required {
        color: red;
        font-weight: bold;
    }
    a.pseudo {
        border-bottom:1px dashed; 
        text-decoration: none;
    }
</style>