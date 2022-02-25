---
title: Reveknew - API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - Example Value | Model
  

  

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>


search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the  Reveknew-API
---

# Introduction

Reveknew provides a simple and powerful REST API to schedule payments and payment reminders for your customers via SMS.
This API reference provides information on the endpoints and how to use them. 
Take note that your business must be marked as an enterprise and be provided with credentials by contacting the sales department of Reveknew.
When enterprise features is provided to you, you will be able to see the businessId that you need in all the API calls.

The Reveknew API can be accessed via the base urls below.

Production base url: `https://api.reveknew.net`


UAT base url: `https://uat.api.reveknew.net`

# Authentication
This API supports this mode of authentication:
* OAuth2 
```
curl https://api.reveknew.net/enterprise/v1/customers/53e08b340cd983/id/f27a7b170cd \
    -H "Authorization: Bearer abd90df5f27a7b170cd775abf89d632b350b7c1c9d53e08b340cd9832ce52c2c"
```

## OAuth2
To make calls to this API, you will need to be authenticated via OAuth2.You will need a clientId and clientSecret from the Enterprise section of the Reveknew dashboard.
You will need to perform the following operation to get an access token.

`POST https://auth.reveknew.net/oauth/token
Content-Type: application/x-www-form-urlencoded
grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&audience=https://services.reveknew.net`


Any further API call now needs to include the access token in the `Authorization` header as a bearer token. To use the bearer token, construct a normal HTTPS request and include an `Authorization` header with the value of Bearer.

# Customers

## Create a customer record under a business account

> **Example Customer body**

```json
 {
    "customerNum" : "4515",
    "email" : "JoeyEGosselin@jourrapide.com",
    "firstName" : "Joey",
    "lastName" : "Gosselin",
    "phoneNo" : "0222740128"
  }
```

This endpoint creates a customer record under a business account

### HTTP Request

`POST`  /enterprise/v1/customers/{businessId}

### Path Parameters

| Parameter            | Description |
|----------------------|-------------|
| `businessId`         | businessId  |
| *string*, *path*     |             |
| `customer`           | customer    |
| *body*               |             |

### Responses

| Code | Description                                    |
|------|------------------------------------------------|
| 200  | Customer record created successfully           |
| 201  | Created                                        |
| 400  | Invalid data supplied for creation of customer |
| 401  | Unauthorized                                   |
| 403  | Operation not permitted for this business      |
| 404  | Invalid businessId supplied                    |



## Update a Customer record under a business account

> **Example Customer body**

```json
  {
    "customerNum" : "77014",
    "email" : "ChristinaRPerrin@jourrapide.com",
    "firstName" : "Christina",
    "id" : "f6867d4c-8a72-11ec-a8a3-0242ac120002",
    "lastName" : "Perrin",
    "phoneNo" : "0515872451"
  }
```

This endpoint updates a customer record under a business account

### HTTP Request

`PUT`  /enterprise/v1/customers/{businessId}

### Path Parameters

| Parameter            | Description |
|----------------------|-------------|
| `businessId`         | businessId  |
| *string*, *path*     |             |
| `customer`           | customer    |
| *body*               |             |

### Responses

| Code | Description                                         |
|------|-----------------------------------------------------|
| 200  | Customer record updated successfully                | 
| 201  | Created                                             |
| 400  | Invalid data supplied for update of customer record |
| 401  | Unauthorized                                        |
| 403  | Operation not permitted for this business           |
| 404  | Invalid businessId supplied                         |

## Find customer record by its id

>**Example Customer response**

```json
  {
    "customerNum": "7784",
    "email": "KimBlake@centme.com",
    "firstName": "Kim",
    "id": "d17ecab2-8a63-11bn-a8a3-8942ac120002",
    "lastName": "Blake",
    "phoneNo": "0552740129"
  }

```

This endpoint finds  customer record by its id 

### HTTP Request

`GET` /enterprise/v1/customers/{businessId}/id/{customerId}

### Path Parameters

| Parameter            | Description |
|----------------------|-------------|
| `businessId`         | businessId  |
| *string*, *path*     |             |
| `customerId`         | customerId  |
| *string*, *path*     |             |

### Responses

| Code | Description                               |
|------|-------------------------------------------|
| 200  | Customer record found successfully        | 
| 401  | Unauthorized                              |
| 403  | Operation not permitted for this business |
| 404  | Invalid businessId supplied               |



# Subscriptions


## Create a subscription record under a business account

### Subscription policy

> **Example Subscription body with SUBSCRIPTION policy**

```json
{
  "customerId": "9e0fb8d9-f7bf-4db0-931b-3e5a04b49c68",
  "tierId": "2fb2a2d4-2454-4a06-8b4b-6fd7c74dd7dc",
  "amount": 20,
  "startDate": "2022-02-25"
}
```

This endpoint creates a subscription record under a business account.
To create a subscription to a tier with policy type being “SUBSCRIPTION”, 
the request body requires the `customerId`, `tierId` and `amount`. 
If `startdate` is not specified, billing will start immediately.

### Request body

|Parameter   | Type   | Required | Description       |
|------------|--------|----------|-------------------|
|`customerId`| string | Required | customerId        |
|`tierId`    | string | Required | tierId            |
|`amount`    | double | Required | amount of payment |
|`startDate` | Date   | Optional | start date        |


### Tier/Schedule policy

> **Example Subscription body with TIER/SCHEDULE policy**

```json
{
  "customerId": "9e0fb8d9-f7bf-4db0-931b-3e5a04b49c68",
  "tierId": "2fb2a2d4-2454-4a06-8b4b-6fd7c74dd7dc",
  "startDate": "2022-02-25"
}
```

To create a subscription to a tier with policy type being “TIER” or “SCHEDULE”, the request body requires the `customerId` and `tierId`. 
If `startdate` is not specified, billing will start immediately. The “startDate” is ignored if the tier has policy type of “SCHEDULE”.


### Request body

|Parameter   | Type   | Required | Description       |
|------------|--------|----------|-------------------|
|`customerId`| string | Required | customerId        |
|`tierId`    | string | Required | tierId            |
|`startDate` | Date   | Optional | start date        |

### HTTP Request

`POST`  /enterprise/v1/subscriptions/{businessId}

### Path Parameters

| Parameter            | Description |
|----------------------|-------------|
| `businessId`         | businessId  |
| *string*, *path*     |             |
| `subscription`       | subscription|
| *body*               |             |

### Responses

| Code | Description                                    |
|------|------------------------------------------------|
| 200  | Subscription record updated successfully       | 
| 201  | Created                                        |
| 400  | Invalid data supplied for creation of customer |
| 401  | Unauthorized                                   |
| 403  | Operation not permitted for this business      |
| 404  | Invalid businessId supplied                    |


## Find a subscription under a business account by id 

> **Example Subscription response**

```json
{
  "amount": 20,
  "customerId": "97f9a74a-9648-11ec-b909-0242ac120002",
  "lastBilledOn": "2022-01-25T07:36:17Z",
  "nextBillingDate": "2022-02-25T07:36:17Z",
  "receipts": [
    {
      "amount": 20,
      "receivedAt": "2022-01-25T07:36:17Z",
	  "subscriptionId": "83fe46e6-445d-4137-b255-daba5c3c8433"
    }
  ],
  "schedules": [
    {
      "amount": 20,
      "graceDate": "2022-01-26",
      "reminderDate": "2022-01-23",
      "scheduledFor": "2022-01-25",
      "shortenedUrl": "https://rvkn.app/daba5c3c8433",
      "status": "COMPLETED",
      "subscriptionId": "83fe46e6-445d-4137-b255-daba5c3c8433"
    },
    {
      "amount": 20,
      "graceDate": "2022-02-26",
      "reminderDate": "2022-02-23",
      "scheduledFor": "2022-02-25",
      "shortenedUrl": "https://rvkn.app/83fe46e6",
      "status": "PENDING",
      "subscriptionId": "83fe46e6-445d-4137-b255-daba5c3c8433"
    }
  ],
  "startDate": "2022-01-25",
  "status": "ACTIVE",
  "tierId": "1b744e0a-8495-4a7d-ba7b-08e330b2ef6f",
  "id": "83fe46e6-445d-4137-b255-daba5c3c8433"
}

```


This endpoint finds a subscription record under a business account by id

### HTTP Request

`GET`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}

### Path Parameters

| Parameter            | Description   |
|----------------------|---------------|
| `businessId`         | businessId    |
| *string*, *path*     |               |
| `subscriptionId`     | subscriptionId|
| *string*, *path*     |               |

### Responses

| Code | Description                                   |
|------|-----------------------------------------------|
| 200  | Subscription record found successfully        |
| 401  | Unauthorized                                  |
| 403  | Operation not permitted for this business     |
| 404  | Invalid businessId or subscriptionId supplied |



## Find a scheduled payment under a business account by id

This endpoint finds a scheduled payment under a business account by id 

### HTTP Request

`GET`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/{scheduleId}

### Path Parameters

| Parameter               | Description         |
|-------------------------|---------------------|
| `businessId`            | businessId          |
|  *string*, *path*       |                     |
| `subscriptionId`        | subscriptionId      |
|  *string*, *path*       |                     |
| `scheduledId`           | scheduleId          |
|  *string*, *path*       |                     |

### Responses

| Code | Description                                                |
|------|------------------------------------------------------------|
| 200  | PaymentSchedule record created successfully                |
| 401  | Unauthorized                                               |
| 403  | Operation not permitted for this business                  |
| 404  | Invalid businessId, subscriptionId or scheduledId supplied |



## Cancel a subscription

This endpoint cancels a subscription record under a business account by id

### HTTP Request

`PATCH`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/cancel

### Path Parameters

| Parameter            | Description   |
|----------------------|---------------|
| `businessId`         | businessId    |
| *string*, *path*     |               |
| `subscriptionId`     | subscriptionId|
| *string*, *path*     |               |

### Responses

| Code | Description                                   |
|------|-----------------------------------------------|
| 200  | Subscription cancelled successfully           |
| 204  | No Content                                    |
| 401  | Unauthorized                                  |
| 403  | Operation not permitted for this business     |
| 404  | Invalid businessId or subscriptionId supplied |



## Pause a subscription

This endpoint pauses a subscription record under a business account 

### HTTP Request

`PATCH`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/pause

### Path Parameters

| Parameter            | Description   |
|----------------------|---------------|
| `businessId`         | businessId    |
| *string*, *path*     |               |
| `subscriptionId`     | subscriptionId|
| *string*, *path*     |               |

### Responses

| Code | Description                                   |
|------|-----------------------------------------------|
| 200  | Subscription paused successfully              |
| 204  | No Content                                    |
| 401  | Unauthorized                                  |
| 403  | Operation not permitted for this business     |
| 404  | Invalid businessId or subscriptionId supplied |



## Update a PaymentSchedule record

> **Example PaymentSchedule body**

```json
  {
    "amount": 74,
    "graceDate": "2020-03-13",
    "id": "6bb42d33-c2a2-4763-90e4-ff1702fc9951",
    "reminderDate": "2020-03-30",
    "scheduledFor": "2020-04-01",
    "shortenedUrl": "https://rvkn.app/3330eb10",
    "status": "AVAILABLE",
    "subscriptionId": "3330eb10-8a8c-11ec-a8a3-0242ac120002"
  }
```

This endpoint updates a PaymentSchedule record
### HTTP Request

`PUT`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/schedule

### Path Parameters

| Parameter            | Description      |
|----------------------|------------------|
| `businessId`         | businessId       |
|  *string*, *path*    |                  |
| `subscriptionId`     | subscriptionId   |
|  *string*, *path*    |                  |
| `updateRequestDto`   | updateRequestDto |
|  *body*              |                  |

### Responses

| Code | Description                                    |
|------|------------------------------------------------|
| 200  | Payment schedule updated successfully          |
| 201  | Created                                        |
| 400  | Invalid data supplied for creation of customer |
| 401  | Unauthorized                                   |
| 403  | Operation not permitted for this business      |
| 404  | Invalid businessId supplied                    |

## Resend SMS notification of payment 

> **Example notification of payment**

```json
  {
    "amount": 64,
    "date": "2017-07-25",
    "id": "3993cf04-8a73-11ec-a8a3-0242ac120002"
  }
```

This endpoint resends SMS notification of payment 

### HTTP Request

`PATCH`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/schedule

### Path Parameters

| Parameter            | Description   |
|----------------------|---------------|
| `businessId`         | businessId    |
| *string*, *path*     |               |
| `subscriptionId`     | subscriptionId|
| *string*, *path*     |               |


### Responses

| Code | Description                                             |
|------|---------------------------------------------------------|
| 200  | SMS notification sent successfully                      |
| 204  | No Content                                              |
| 400  | Invalid data supplied for creation of payment schedules |
| 401  | Unauthorized                                            |
| 403  | Operation not permitted for this business               |
| 404  | Invalid businessId or subscriptionId supplied           |



## Schedule payments for this subscription using the dates and amounts specified in the body 

This endpoint schedule payments for this subscription using the dates and amounts specified in the body

### HTTP Request

`POST`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/schedule/date

### Path Parameters

| Parameter            | Description      |
|----------------------|------------------|
| `businessId`         | businessId       |
|  *string*, *path*    |                  |
| `subscriptionId`     | subscriptionId   |
|  *string*, *path*    |                  |
| `payments`           | payments         |
|  *body*              |                  |

### Responses

| Code | Description                                    |
|------|------------------------------------------------|
| 200  | Payment schedule generated successfully        |
| 201  | Created                                        |
| 400  | Invalid data supplied for creation of customer |
| 401  | Unauthorized                                   |
| 403  | Operation not permitted for this business      |
| 404  | Invalid businessId supplied                    |

## Schedule payments for this subscription using the order and amounts specified in the body

> **Example request**

```json
  {
    "amount": 27,
    "order": 0
  }
```

This endpoint schedule payments for this subscription using the order and amounts specified in the body


### HTTP Request

`POST`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/schedule/order

### Path Parameters

| Parameter            | Description      |
|----------------------|------------------|
| `businessId`         | businessId       |
|  *string*, *path*    |                  |
| `subscriptionId`     | subscriptionId   |
|  *string*, *path*    |                  |
| `payments`           | payments         |
|  *body*              |                  |

### Responses

| Code | Description                                             |
|------|---------------------------------------------------------|
| 200  | Payments schedules cancelled successfully               |
| 201  | Created                                                 |
| 400  | Invalid data supplied for creation of payment schedules |
| 401  | Unauthorized                                            |
| 403  | Operation not permitted for this business               |
| 404  | Invalid businessId or subscriptionId supplied           |

## Find all subscriptions for a customer by customerId

This endpoint finds all subscriptions for a customer by customerId

### HTTP Request

`GET`  /enterprise/v1/subscriptions/{businessId}/customer/{customerId}

### Path Parameters

| Parameter            | Description |
|----------------------|-------------|
| `businessId`         | businessId  |
| *string*, *path*     |             |
| `customerId`         | customerId  |
| *string*, *path*     |             |

### Responses

| Code | Description                                   |
|------|-----------------------------------------------|
| 200  | Customer subscriptions found successfully     |
| 401  | Unauthorized                                  |
| 403  | Operation not permitted for this business     |
| 404  | Invalid businessId or subscriptionId supplied |

# Tiers

## Create a tier under a business account

> **Example Tier body**

```json
  {
    "amount": 10,
    "billingPeriod": "ONCE",
    "deductions": 0,
    "graceDays": 0,
    "name": "Basic",
    "policy": "TIER",
    "reminderDays": 2
  }
```

This endpoint creates a tier under a business account


### HTTP Request

`POST`  /enterprise/v1/tiers/{businessId}

### Path Parameters

| Parameter            | Description |
|----------------------|-------------|
| `businessId`         | businessId  |
| *string*, *path*     |             |
| `tier`               | tier        |
| *body*               |             |

### Details on Tier policy types

| Policy Type    | Description                                                                                                                                    |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|`TIER`          | This deduction policy means the amount to deduct is fixed for every customer. This is the default                                              |
|`SUBSCRIPTION`  | This deduction policy means the amount to deduct is defined at the time of creating a subscription. Example is a fixed life insurance deduction|
|`SCHEDULE`      | This means the amount to deduct is defined at the time of creating a payment schedule. Example is a loan with different repayments every month.|


### Responses

| Code | Description                                |
|------|--------------------------------------------|
| 200  | Tier created successfully                  |
| 201  | Created                                    |
| 400  | Invalid data supplied for creation of tier |
| 401  | Unauthorized                               |
| 403  | Operation not permitted for this business  |
| 404  | Invalid businessId or  supplied            |

## Get a tier record by its id

>**Example Tier response**

```json
  {
    "amount": 35,
    "billingPeriod": "ONCE",
    "deductions": 0,
    "graceDays": 0,
    "id": "576f3640-90a3-11ec-b909-0242ac120002",
    "name": "Special Packages",
    "policy": "TIER",
    "reminderDays": 2
  }
```


This endpoint gets a tier record by its id

### HTTP Request

`GET` /enterprise/v1/tiers/{businessId}/{tierId}

### Path Parameters

| Parameter            | Description |
|----------------------|-------------|
| `businessId`         | businessId  |
| *string*, *path*     |             |
| `tierId`             | tierId      |
| *string*, *path*     |             |

### Responses

| Code | Description                               |
|------|-------------------------------------------|
| 200  | Tier found by id                          |
| 401  | Unauthorized                              |
| 403  | Operation not permitted for this business |
| 404  | Invalid businessId or tierId supplied     |

## Update a tier under a business account

> **Example Tier body**

```json
 {
    "amount": 48,
    "billingPeriod": "ONCE",
    "deductions": 0,
    "description": "Enjoy new additional services",
    "graceDays": 0,
    "id": "6325a978-8a73-11ec-a8a3-0242ac120002",
    "name": "Premium",
    "policy": "TIER",
    "reminderDays": 2
  }

```

This endpoint updates a tier under a business account


### HTTP Request

`PUT`  /enterprise/v1/tiers/{businessId}/{tierId}

### Path Parameters

| Parameter            | Description |
|----------------------|-------------|
| `businessId`         | businessId  |
| *string*, *path*     |             |
| `tierId`             | tierId      |
| *string*, *path*     |             |
| `tier`               | tier        |
| *body*               |             |

### Responses

| Code | Description                                |
|------|--------------------------------------------|
| 200  | Tier updated successfully                  |
| 201  | Created                                    |
| 400  | Invalid data supplied for creation of tier |
| 401  | Unauthorized                               |
| 403  | Operation not permitted for this business  |
| 404  | Invalid businessId or tierId supplied      |
