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

The Reveknew API can be accessed via the base url below.

`https://api.reveknew.net`

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

This endpoint creates a customer record under a business account

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

### HTTP Request

`POST`  /enterprise/v1/customers/{businessId}

### Path Parameters

| Parameter            | Description                             |
|----------------------|-----------------------------------------|
| `businessId`         | The unique account key of the business  |
| *string*, *path*     |                                         |
| `customer`           | The customer of the business            |
| *body*               |                                         |

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

This endpoint updates a customer record under a business account


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

### HTTP Request

`PUT`  /enterprise/v1/customers/{businessId}

### Path Parameters

| Parameter            | Description                             |
|----------------------|-----------------------------------------|
| `businessId`         | The unique account key of the business  |
| *string*, *path*     |                                         |
| `customer`           | The customer of the business            |
| *body*               |                                         |

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

This endpoint finds  customer record by its id


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

### HTTP Request

`GET` /enterprise/v1/customers/{businessId}/id/{customerId}

### Path Parameters

| Parameter            | Description                             |
|----------------------|-----------------------------------------|
| `businessId`         | The unique account key of the business  |
| *string*, *path*     |                                         |
| `customerId`         | The unique identifier of the customer   |
| *string*, *path*     |                                         |

### Responses

| Code | Description                               |
|------|-------------------------------------------|
| 200  | Customer record found successfully        | 
| 401  | Unauthorized                              |
| 403  | Operation not permitted for this business |
| 404  | Invalid businessId supplied               |



# Subscriptions


## Create a subscription record under a business account

This endpoint creates a new subscription record under a business account.

> **Example Subscription response for creating a new subscription**

```json
{
  "id": "83fe46e6-445d-4137-b255-daba5c3c8433",
  "amount": 20,
  "customerId": "97f9a74a-9648-11ec-b909-0242ac120002",
  "nextBillingDate": "2022-02-25T07:36:17Z",
  "schedules": [
    {
      "amount": 20,
      "graceDate": "2022-02-26",
      "reminderDate": "2022-02-23",
      "scheduledFor": "2022-02-25",
      "shortenedUrl": "https://rvkn.app/83fe46e6",
      "status": "PENDING"
    }
  ],
  "startDate": "2022-01-25",
  "status": "ACTIVE",
  "tierId": "1b744e0a-8495-4a7d-ba7b-08e330b2ef6f"
}
```

### HTTP Request

`POST`  /enterprise/v1/subscriptions/{businessId}

### Path Parameters

| Parameter            | Description                                 |
|----------------------|---------------------------------------------|
| `businessId`         | The unique account key of the business      |
| *string*, *path*     |                                             |
| `subscription`       | Details of subscription from the customer   |
| *body*               |                                             |


### Subscription policy

> Example Subscription body for a tier whose policy value is **SUBSCRIPTION**

```json
{
  "customerId": "9e0fb8d9-f7bf-4db0-931b-3e5a04b49c68",
  "tierId": "2fb2a2d4-2454-4a06-8b4b-6fd7c74dd7dc",
  "amount": 20,
  "startDate": "2022-02-25"
}
```

To create a subscription to a tier with policy type being **SUBSCRIPTION**, 
the request body requires the `customerId`, `tierId` and `amount`. 
If `startdate` is not specified, billing will start immediately.

### Request body

|Parameter   | Type   | Required | Description                                                  |
|------------|--------|----------|--------------------------------------------------------------|
|`customerId`| string | Required | Unique identifier of the customer who owns the subscription  |
|`tierId`    | string | Required | The id of the tier                                           |
|`amount`    | double | Required | Amount of payment                                            |
|`startDate` | Date   | Optional | The date the subscription started                            |


### Tier/Schedule policy

> Example Subscription body for a tier whose policy value is either **TIER**/**SCHEDULE**

```json
{
  "customerId": "9e0fb8d9-f7bf-4db0-931b-3e5a04b49c68",
  "tierId": "2fb2a2d4-2454-4a06-8b4b-6fd7c74dd7dc",
  "startDate": "2022-02-25"
}
```

To create a subscription to a tier with policy type being **TIER** or **SCHEDULE**, the request body requires the `customerId` and `tierId`. 
If `startdate` is not specified, billing will start immediately. The `startDate` is ignored if the tier has policy type of **SCHEDULE**.


### Request body

|Parameter   | Type   | Required | Description                                                  |
|------------|--------|----------|--------------------------------------------------------------|
|`customerId`| string | Required |  Unique identifier of the customer who owns the subscription |
|`tierId`    | string | Required |  The id of the tier                                          |
|`startDate` | Date   | Optional |  The date the subscription started                           |


### Responses

| Code | Description                                                 |
|------|-------------------------------------------------------------|
| 200  | Subscription record updated successfully                    |  
| 201  | Created                                                     |
| 400  | Invalid data supplied for creation of a subscription record |
| 401  | Unauthorized                                                |
| 403  | Operation not permitted for this business                   |
| 404  | Invalid businessId supplied                                 | 


## Find a subscription under a business account by id 

This endpoint finds a subscription record under a business account by id


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

### HTTP Request

`GET`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}

### Path Parameters

| Parameter            | Description                           |
|----------------------|---------------------------------------|
| `businessId`         | Unique account key of the business    |
| *string*, *path*     |                                       |
| `subscriptionId`     | The id of the subscription            |
| *string*, *path*     |                                       |

### Responses

| Code | Description                                   |
|------|-----------------------------------------------|
| 200  | Subscription record found successfully        |
| 401  | Unauthorized                                  |
| 403  | Operation not permitted for this business     |
| 404  | Invalid businessId or subscriptionId supplied |



## Find a scheduled payment under a business account by id

This endpoint finds a scheduled payment under a business account by id 

> **Example scheduled payment response**

```json
{
  "id": "951a9cb4-9a26-11ec-b909-0242ac120002",
  "subscriptionId": "a0436d32-9a26-11ec-b909-e08a7c183209",
  "scheduledFor": "2022-03-27",
  "reminderDate": "2022-03-26",
  "graceDate": "2022-03-28",
  "amount": 50,
  "status": "SUSPENDED",
  "shortenedUrl": "https://rvkn.app/e08a7c183209"
}
```

### HTTP Request

`GET`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/{scheduleId}

### Path Parameters

| Parameter               | Description                                                                       |
|-------------------------|-----------------------------------------------------------------------------------|
| `businessId`            | Unique account key of the business                                                |
|  *string*, *path*       |                                                                                   |
| `subscriptionId`        | The id of the subscription                                                        |
|  *string*, *path*       |                                                                                   |
| `scheduleId`            | The id of the schedule that allows you to manage the lifecycle of the subscription|                               
|  *string*, *path*       |                                                                                   |

### Responses

| Code | Description                                                |
|------|------------------------------------------------------------|
| 200  | PaymentSchedule record created successfully                |
| 401  | Unauthorized                                               |
| 403  | Operation not permitted for this business                  |
| 404  | Invalid businessId, subscriptionId or scheduleId supplied |



## Cancel a subscription

This endpoint cancels a subscription record under a business account by id

### HTTP Request

`PATCH`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/cancel

### Path Parameters

| Parameter            | Description                        |
|----------------------|------------------------------------|
| `businessId`         | Unique account key of the business |
| *string*, *path*     |                                    |
| `subscriptionId`     | The id of the subscription         |
| *string*, *path*     |                                    |

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

| Parameter            | Description                           |
|----------------------|---------------------------------------|
| `businessId`         | Unique account key of the business    |
| *string*, *path*     |                                       |
| `subscriptionId`     | The id of the subscription            |
| *string*, *path*     |                                       |

### Responses

| Code | Description                                   |
|------|-----------------------------------------------|
| 200  | Subscription paused successfully              |
| 204  | No Content                                    |
| 401  | Unauthorized                                  |
| 403  | Operation not permitted for this business     |
| 404  | Invalid businessId or subscriptionId supplied |



## Update a scheduled subscription payment

This endpoint updates a PaymentSchedule record


> **Example PaymentSchedule body**

```json
{
  "id": "b546a9cb4-9a26-11ec-b909-0242ac5562b9",
  "amount": 42,
  "date": "2022-08-17"
}
```

> **Example PaymentSchedule response**

```json
{
  "id": "b546a9cb4-9a26-11ec-b909-0242ac5562b9",
  "subscriptionId": "c0e32-9a26-11ec-b909-e08a7c1955",
  "scheduledFor": "2022-08-17",
  "reminderDate": "2022-03-16",
  "graceDate": "2022-03-18",
  "amount": "42",
  "status": "ACTIVE",
  "shortenedUrl": "https://rvkn.app/0242ac5562b9"
}
```


### HTTP Request

`PUT`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/schedule

### Path Parameters

| Parameter            | Description                         |
|----------------------|-------------------------------------|
| `businessId`         | Unique account key of the business  |
|  *string*, *path*    |                                     |
| `subscriptionId`     | The id of the subscription          |
|  *string*, *path*    |                                     |
| `payload`            | The body of the request             |
|  *body*              |                                     |

### Responses

| Code | Description                                                         |
|------|---------------------------------------------------------------------|
| 200  | Payment schedule updated successfully                               |
| 201  | Created                                                             |
| 400  | Invalid data supplied for updating a scheduled subscription payment |
| 401  | Unauthorized                                                        |
| 403  | Operation not permitted for this business                           |
| 404  | Invalid businessId supplied                                         |

## Resend SMS notification of payment 

This endpoint resends SMS notification of payment


> **Example notification of payment**

```json
  {
    "amount": 64,
    "date": "2017-07-25",
    "id": "3993cf04-8a73-11ec-a8a3-0242ac120002"
  }
```

### HTTP Request

`PATCH`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/schedule

### Path Parameters

| Parameter            | Description                         |
|----------------------|-------------------------------------|
| `businessId`         | Unique account key of the business  |
| *string*, *path*     |                                     |
| `subscriptionId`     | The id of the subscription          |
| *string*, *path*     |                                     |


### Responses

| Code | Description                                             |
|------|---------------------------------------------------------|
| 200  | SMS notification sent successfully                      |
| 204  | No Content                                              |
| 400  | Invalid data supplied for sending SMS notification      |
| 401  | Unauthorized                                            |
| 403  | Operation not permitted for this business               |
| 404  | Invalid businessId or subscriptionId supplied           |



## Schedule payments for this subscription using the dates and amounts specified in the body 

This endpoint schedule payments for this subscription using the dates and amounts specified in the body

> **Example payment scheduling body**

```json
[
  {
     "date": "2022-04-30",
     "amount": 30
  },
  {
     "date": "2022-06-30",
     "amount": 20
  },
  {
     "date": "2022-05-30",
     "amount": 25
  }
]

```

### HTTP Request

`POST`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/schedule/date

### Path Parameters

| Parameter            | Description                                              |
|----------------------|----------------------------------------------------------|
| `businessId`         | Unique account key of the business                       |
|  *string*, *path*    |                                                          |
| `subscriptionId`     | The id of the subscription                               |
|  *string*, *path*    |                                                          |
| `payments`           | The payments made by the customer for the subscription   |
|  *body*              |                                                          |

### Responses

| Code | Description                                                           |
|------|-----------------------------------------------------------------------|
| 200  | Payment schedule generated successfully                               |
| 201  | Created                                                               |
| 400  | Invalid data supplied for creation of a scheduled subscription payment| 
| 401  | Unauthorized                                                          |
| 403  | Operation not permitted for this business                             |
| 404  | Invalid businessId or subscriptionId supplied                         |

## Schedule payments for this subscription using the order and amounts specified in the body

This endpoint schedule payments for this subscription using the order and amounts specified in the body

> **Example request**

```json
  {
    "amount": 27,
    "order": 0
  }
```

### HTTP Request

`POST`  /enterprise/v1/subscriptions/{businessId}/{subscriptionId}/schedule/order

### Path Parameters

| Parameter            | Description                                             |
|----------------------|---------------------------------------------------------|
| `businessId`         | Unique account key of the business                      |
|  *string*, *path*    |                                                         |
| `subscriptionId`     | The id of the subscription                              |
|  *string*, *path*    |                                                         |
| `payments`           | The payments made by the customer for the subscription  |
|  *body*              |                                                         |

### Responses

| Code | Description                                               |
|------|-----------------------------------------------------------|
| 200  | Payments schedules cancelled successfully                 | 
| 201  | Created                                                   |
| 400  | Invalid data supplied for cancelling of payment schedules |
| 401  | Unauthorized                                              |
| 403  | Operation not permitted for this business                 |
| 404  | Invalid businessId or subscriptionId supplied             |

## Find all subscriptions for a customer by customerId

This endpoint finds all subscriptions for a customer by customerId

> **Example subscription response of a customer with two subscriptions** 

```json

{
  "customerId": "dccdbc81-91bc-4a1f-9eb2-078283fb835b",
  "subscriptions": [
    {
      "id": "9d37205c-9ed1-11ec-b909-0242ac120002",
      "amount": 10,
      "nextBillingDate": "2022-04-30T10:30:18Z",
      "schedules": 
        [
          {
            "date": "2022-04-30",
            "amount": 30
          },
          {
            "date": "2022-05-30",
            "amount": 25
          },
          {
            "date": "2022-06-30",
            "amount": 20
          }
        ],
      "startDate": "2022-04-30",
      "status": "ACTIVE",
      "tierId": "9d37205c-9ed1-11ec-b909-0242ac120002"
    },
    {
      "id": "c2322093-7b47-498b-8f52-a61eac64046b",
      "amount": 15,
      "nextBillingDate": "2022-05-30T07:14:29Z",
      "schedules":
        [
          {
            "date": "2022-05-30",
            "amount": 25
          },
          {
            "date": "2022-06-30",
            "amount": 20
          },
          {
            "date": "2022-07-30",
            "amount": 10
          }
        ],
      "startDate": "2022-05-30",
      "status": "ACTIVE",
      "tierId": "397bd03f-6c52-4fd9-9607-be8c2b34cd9e"
    }
  ]
}
```


### HTTP Request

`GET`  /enterprise/v1/subscriptions/{businessId}/customer/{customerId}

### Path Parameters

| Parameter            | Description                                                      |
|----------------------|------------------------------------------------------------------|
| `businessId`         | Unique account key of the business                               |
| *string*, *path*     |                                                                  |
| `customerId`         | The unique identifier of the customer who owns the subscription  |
| *string*, *path*     |                                                                  |

### Responses

| Code | Description                                   |
|------|-----------------------------------------------|
| 200  | Customer subscriptions found successfully     |
| 401  | Unauthorized                                  |
| 403  | Operation not permitted for this business     |
| 404  | Invalid businessId or subscriptionId supplied |

# Tiers

## Create a tier under a business account

This endpoint creates a tier under a business account


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

### HTTP Request

`POST`  /enterprise/v1/tiers/{businessId}

### Path Parameters

| Parameter            | Description                                       |
|----------------------|---------------------------------------------------|
| `businessId`         | Unique account key of the business                |
| *string*, *path*     |                                                   |
| `tier`               | Details of individual payment plans for customers |
| *body*               |                                                   |

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

This endpoint gets a tier record by its id

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

### HTTP Request

`GET` /enterprise/v1/tiers/{businessId}/{tierId}

### Path Parameters

| Parameter            | Description                         |
|----------------------|-------------------------------------|
| `businessId`         | Unique account key of the business  |
| *string*, *path*     |                                     |
| `tierId`             | The id of the tier                  |
| *string*, *path*     |                                     |

### Responses

| Code | Description                               |
|------|-------------------------------------------|
| 200  | Tier found by id                          |
| 401  | Unauthorized                              |
| 403  | Operation not permitted for this business |
| 404  | Invalid businessId or tierId supplied     |

## Update a tier under a business account

This endpoint updates a tier under a business account

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

### HTTP Request

`PUT`  /enterprise/v1/tiers/{businessId}/{tierId}

### Path Parameters

| Parameter            | Description                                       |
|----------------------|---------------------------------------------------|
| `businessId`         | Unique account key of the business                |
| *string*, *path*     |                                                   |
| `tierId`             | The id of the tier                                |
| *string*, *path*     |                                                   |
| `tier`               | Details of individual payment plans for customers |
| *body*               |                                                   |

### Responses

| Code | Description                                |
|------|--------------------------------------------|
| 200  | Tier updated successfully                  |
| 201  | Created                                    |
| 400  | Invalid data supplied for update of tier   |
| 401  | Unauthorized                               |
| 403  | Operation not permitted for this business  |
| 404  | Invalid businessId or tierId supplied      |
