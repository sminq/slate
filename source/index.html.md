---
title: API Reference

language_tabs:
  - shell
 
toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Sminq API! 
<aside class="notice">
An authorization key will be required to test the api calls.
</aside>


# Business
## Login

> Business login :


```shell
curl "https://api.sminq.com/v1/business/login"
  -H "Authorization: XXXXXXXXXX"
```

> The above api returns JSON structured like this if OTP is required:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "otp": {
      "password": "XjartdfgFpj6rYV/3oK0zQ==",
      "mobile": "9393939393",
    },
    "verified": false,
  }
}
```
> The above api returns JSON structured like this if PIN is provided:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "sessionKey": "IEEJdv/UnoQTZyhqoFhooOHwi65TEW+APGJ4aMwdLfD/9O1njWCbAIxiwJxxse/RgfVqgKuPvTXcrAXl3dnTFCo+dB3L0yLj/jmZLUtKr9Vt3/m6IPNQqzC4P2FO99Cv",
    "offlineDataUrl": "https://d35hf7a7nc5458.cloudfront.net/offline/",
    "verified": true,
    "business": {
      "businessId": 14,
      "businessName": "SminqIndia",
      "businessLatitude": 18.519489464507107,
      "businessLongitude": 73.90624713897705,
      "mobile": "9393939393",
      "countryCode": "+91",
      "contactId": 13,
      "address": "Pune",
      "cityName": "Pune",
      "cityId": 1,
      "verticalType": "clinic"
    }
  }
}

```

This endpoint retrieves business details after a successful login.

### HTTP Request

`POST http://api.sminq.com/v1/business/login`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | 9xxxxxxxxxx | The business primary contact.
isSmsOtp | 1 | If set to 1, sms will be sent with OTP for verification.
pin | null | If value is set, then sms verification will be skipped.

### Error codes

Code | Description
--------- | -----------
111 |  Invalid PIN.
100 |  This mobile is not registered.

## Verify

> Business login verification, if OTP is enabled:


```shell
curl "https://api.sminq.com/v1/business/verify"
  -H "Authorization: XXXXXXXXXX"
```

> The above api returns JSON structured like this if OTP is correct:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "sessionKey": "IEEJdv/UnoQTZyhqoFhooOHwi65TEW+APGJ4aMwdLfD/9O1njWCbAIxiwJxxse/RgfVqgKuPvTXcrAXl3dnTFCo+dB3L0yLj/jmZLUtKr9Vt3/m6IPNQqzC4P2FO99Cv",
    "offlineDataUrl": "https://d35hf7a7nc5458.cloudfront.net/offline/",
    "verified": true,
    "business": {
      "businessId": 14,
      "businessName": "SminqIndia",
      "businessLatitude": 18.519489464507107,
      "businessLongitude": 73.90624713897705,
      "mobile": "9393939393",
      "countryCode": "+91",
      "contactId": 13,
      "address": "Pune",
      "cityName": "Pune",
      "cityId": 1,
      "verticalType": "clinic"
    }
  }
}

```

This endpoint retrieves business details after a successful verification.

### HTTP Request

`POST http://api.sminq.com/v1/business/login`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | 9xxxxxxxxxx | The business primary contact.
password | xxxxxx | 4-6 digit OTP for verification.


### Error codes

Code | Description
--------- | -----------
100 |  This mobile is not registered.
301 |  One time password verification failed.

## Create Pin

> Business can create a PIN to login, to avoid OTP verification on every login.

```shell
curl "http://api.sminq.com/v1/business/pin/generate"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "mobile": "9797979798",
    "pin": "F1JhJzIT6EkTf+saemUtRQ==",
    "password": null
  }
}
```

This endpoint sets PIN for business .

### HTTP Request

`POST http://api.sminq.com/v1/business/pin/generate`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | true | Registered user mobile.
pin | true | User 4 digit pin.

### Error codes

Code | Description
--------- | -----------
337 |  Business mobile cannot be empty.
100 |  Business is not registered.

## Device Register

> this will register business device for future push notifications


```shell
curl "http://api.sminq.com/v1/business/device/register"
  -H "Authorization: xxxxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "id": 1,
    "businessId": 1,
    "deviceType": "android",
    "registrationId": "ZElwXzU1Z29JZU06QVBBOTFiRTlHQ0MtVk5tMzNyWUlYQjZ4ckU3U2xUaTdIT0RVd0oweF9DaU1QbC1JbGlEWUFKRGJvaTdUMHFWX1pRVDhZS1A4RGdmUkZOdmMxZXRNTkU0TnNDbjBISHQ1QktreS1XX2xScmpXdkpweWpyVThfRHdvZXJ1c25MQjhUck1kYWNuQktvN18=",
    "deviceEndpointArn": "YXJuOmF3czpzbnM6YXAtc291dGhlYXN0LTE6Mjg4Nzg5MzU0MjQ4OmVuZHBvaW50L0dDTS9TbWlucUJ1c2luZXNzQXBwLzJlMTYxNWM5LTBmNzUtM2I4YS05Njg1LWIxNjk0ZTAwMDliMw==",
    "deviceStatus": 1
  }
}
```

This endpoint registers a business device for push notifications.

### HTTP Request

`POST http://api.sminq.com/v1/business/device/register`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
businessId | true | Unique ID for business.
deviceType | true | values can be "android" or "ios".
registrationId | true | values returned from GCM or APNS.


### Error codes

Code | Description
--------- | -----------
101 |  Invalid business ID.
303 |  Registration ID cannot be empty.


## Device Un-Register

> unregister business device for notifications

```shell
curl "http://api.sminq.com/v1/business/device/unregister"
  -H "Authorization: xxxxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": null
}
```

### HTTP Request

`POST http://api.sminq.com/v1/business/device/unregister`

### Query Parameters

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
businessId | true | Unique ID for business.
deviceType | true | values can be "android" or "ios".
registrationId | true | values returned from GCM or APNS.


### Error codes

Code | Description
--------- | -----------
101 |  Invalid business ID.
303 |  Registration ID cannot be empty.


## Queue search

> Get queues for a business


```shell
curl "http://api.sminq.com/v1/business/queue/search"
  -H "Authorization: xxxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "businessQueues": [
      {
        "queueId": 1,
        "queueName": "ENT",
        "queueStatus": 1,
        "queueConfigId": 1,
        "avgWaitTime": 0,
        "enableAdvanceBooking": 1,
        "enableAdvanceBookingForToday": 0,
        "advanceBookingDays": 7,
        "peopleAheadOfYou": 13,
        "nextSlot": "12:00:00",
        "subQueues": 0,
        "isConfirmed": 0,
        "enablePayments": 0,
        "forcePayment": 1,
        "weeklyAvgWaitTime": 0
      }
    ],
    "action": [
      {
        "name": "ON_JOIN_NOW",
        "form": {
          "field": {
            "name": "Notes",
            "type": "text",
            "label": "Illness, Fever, Follow up, Vaccination..",
            "size": 50,
            "required": false
          }
        }
      }
    ]
  }
}
```

This endpoint retrieves all queues for a business.

### HTTP Request

`GET http://api.sminq.com/v1/business/queue/search`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
businessId | true | Unique ID for business.
queueAccess | false | Permissions to restrict queue access for business.
parentQueue | false | 


## Queue status

> Change status for Queue:

```shell
curl "http://api.sminq.com/v1/business/queue/status"
  -H "Authorization: xxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "statusType": "Q_OPEN",
    "queueId": 1,
    "totalWaitTime": 0,
  }
}
```

This endpoint sets the status for the queue.

### HTTP Request

`POST http://api.sminq.com/v1/business/queue/status`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | The unique queueId for which status needs to be set.
statusType | true | Can be Q_OPEN, Q_CLOSE.

### Error codes

Code | Description
--------- | -----------
308 |  Invalid queue status type.
109 |  StatusType cannot be empty.
102 |  Invalid queue ID.

## Queue calendar

> Get the queue calendar for date:


```shell
curl "http://api.sminq.com/v1/business/queue/calendar"
  -H "Authorization: xxxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "tokenQueueId": 1,
      "tokenId": 2,
      "appType": 1,
      "userId": 11,
      "userName": "demo17",
      "userEmail": null,
      "userMobile": "3616483770",
      "userGroupMobile": null,
      "userGroupName": null,
      "userCityId": 1,
      "tokenNumber": 1102,
      "queueName": "ENT",
      "businessName": "Sahyadri Clinic",
      "statusType": "T_CREATE",
      "joinDate": "2016-08-16",
      "joinTime": "11:10:00",
      "tokenMetadata": null,
      "slotGroupName": "Morning",
      "slotGroup": "09:00:00 - 12:55:00",
      "billAmount": null,
      "refundAmount": null,
      "billingType": null,
      "billingId": null,
      "isPaid": null,
      "clientUuid": "9a6ff998-f085-4183-b2c1-067e7a6d3001",
      "enablePayments": 0,
      "forcePayment": 1
    },
    {
      "tokenQueueId": 1,
      "tokenId": 3,
      "appType": 1,
      "userId": 14,
      "userName": "demo18",
      "userEmail": null,
      "userMobile": "7805802928",
      "userGroupMobile": null,
      "userGroupName": null,
      "userCityId": 1,
      "tokenNumber": 1103,
      "queueName": "ENT",
      "businessName": "Sahyadri Clinic",
      "statusType": "T_CREATE",
      "joinDate": "2016-08-16",
      "joinTime": "11:10:00",
      "tokenMetadata": null,
      "slotGroupName": "Morning",
      "slotGroup": "09:00:00 - 12:55:00",
      "billAmount": null,
      "refundAmount": null,
      "billingType": null,
      "billingId": null,
      "isPaid": null,
      "clientUuid": "8e76d11f-45db-4780-810f-93734064c423",
      "enablePayments": 0,
      "forcePayment": 1
    }
  ]
}
```

This endpoint retrieves the queue calendar to view appointments.

### HTTP Request

`GET http://api.sminq.com/v1/business/queue/calendar`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique queueId .
type | true | available values "live" for live appointments, "upcoming" for future appointments.
limit | false | for pagination
page | false | for pagination

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.

## Get alerts

> Get all alerts set for a queue:

```shell
curl "http://api.sminq.com/v1/business/queue/alert"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "id": 3,
      "queueId": 1,
      "message": "Dr will not be available today, you can take appointments with Assistant",
      "liveDate": "2016-08-16",
      "endDate": "2016-08-17",
      "startTime": null,
      "endTime": null,
      "status": 1,
      "stopQueue": 0,
      "allDay": 0,
      "sendToExisting": null
    }
  ]
}
```

This endpoint retrieves all alerts set for a business queue.

### HTTP Request

`GET http://api.sminq.com/v1/business/queue/alert`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | unique business queue ID.


## Add alert

> Add a queue alert to inform users:


```shell
curl "http://api.sminq.com/v1/business/queue/alert"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "id": 3,
    "queueId": 1,
    "message": "Dr will not be available today, you can take appointments with Assistant",
    "liveDate": "2016-08-16",
    "endDate": "2016-08-17",
    "startTime": null,
    "endTime": null,
    "status": 1,
    "stopQueue": 0,
    "allDay": 0,
    "sendToExisting": null
  }
}
```

This endpoint adds an alert for business queue.

### HTTP Request

`POST http://api.sminq.com/v1/business/queue/alert`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique queue ID for business.
liveDate | true | The date alert should be activated.
endDate | true | The date alert should be de-activated.
allDay | true | set to 1 for all day alert.
startTime | false | set time of the day alert should be active.
endTime | false | set time of the day alert should be active.
stopQueue | false | if set to 1 block new appointments.
sendToExisting | false | Alert users who have already taken appointment.

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.
314 | Alert message cannot be empty
315 | please enter correct date range

## Update alert

> Update an alert set for a queue:


```shell
curl "http://api.sminq.com/v1/business/queue/alert/3"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "queueId": 1,
    "message": "Dr will not be available today, you can take appointments with Assistant",
    "liveDate": "2016-08-16",
    "endDate": "2016-08-16",
    "startTime": null,
    "endTime": null,
    "status": null,
    "stopQueue": 0,
    "allDay": 1,
    "sendToExisting": null
  }
}
```

This endpoint retrieves all kittens.

### HTTP Request

`POST http://api.sminq.com/v1/business/queue/alert/{id}`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique queue ID for business.
liveDate | true | The date alert should be activated.
endDate | true | The date alert should be de-activated.
allDay | true | set to 1 for all day alert.
startTime | false | set time of the day alert should be active.
endTime | false | set time of the day alert should be active.
stopQueue | false | if set to 1 block new appointments.
sendToExisting | false | Alert users who have already taken appointment.

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.
314 | Alert message cannot be empty
315 | please enter correct date range

## Appointment notify

> Notify a user about update to appointment

```shell
curl "http://api.sminq.com/v1/appointment/notify"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "tokenId": 10,
    "peopleAhead": 3,
    "queueId": 1
  }
}
```

This endpoint send a notification to a user who has an appointment.

### HTTP Request

`POST http://api.sminq.com/v1/appointment/notify`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
tokenId | true | Unique appointment ID.
queueId | true | Unique business queue ID.
peopleAhead | true | notify how many people ahead

### Error codes

Code | Description
--------- | -----------
103 |  Invalid token ID.

##Group appointment cancel/close

```shell
curl "http://api.sminq.com/v1/appointment/status/all"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "queueId": 1,
    "statusType": "T_CANCEL",
    "queueId": 1,
    "date": '2016-8-16'
  }
}
```

This endpoint changes status for a group of tokens.

### HTTP Request

`POST http://api.sminq.com/v1/appointment/status/all`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
statusType | true | T_CANCEL or T_CLOSED.
date | true | date for which appointments need to be updated

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.
109 |  Invalid statusType.

## Get appointment summary

```shell
curl "http://api.sminq.com/v1/business/reports/summary"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "queueId": null,
    "joinDate": null,
    "totalClosed": 1,
    "totalCancelled": 2,
    "totalTokens": 24,
    "availableTimeSlot": null,
    "peopleAhead": null,
    "totalPresent": 0,
    "totalAbsent": 0,
    "totalCash": 0,
    "weeklyAvgWaitTime": null,
    "totalOnline": 0
  }
}
```

This endpoint retrieves the appointment summary for a date.

### HTTP Request

`GET http://api.sminq.com/v1/business/reports/summary`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
date | true | date to fetch report for.

## Get appointment report

```shell
curl "http://api.sminq.com/v1/business/reports/appointments"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "tokenQueueId": 1,
      "tokenId": 1,
      "appType": 1,
      "userId": 7,
      "userName": "demo16",
      "userEmail": null,
      "userMobile": "6552853332",
      "userGroupMobile": null,
      "userGroupName": null,
      "userCityId": 1,
      "tokenNumber": 1101,
      "queueName": "ENT",
      "businessName": "Sahyadri Clinic",
      "statusType": "T_CLOSED",
      "joinDate": "2016-08-16",
      "joinTime": "11:10:00",
      "tokenMetadata": null,
      "slotGroupName": "Morning",
      "slotGroup": "09:00:00 - 12:55:00",
      "billAmount": null,
      "refundAmount": null,
      "billingType": null,
      "billingId": null,
      "isPaid": null,
      "clientUuid": "eda6ae1e-10f7-437c-801d-fe65465d41e3",
      "enablePayments": 0,
      "forcePayment": 1
    },
    {
      "tokenQueueId": 1,
      "tokenId": 2,
      "appType": 1,
      "userId": 11,
      "userName": "demo17",
      "userEmail": null,
      "userMobile": "3616483770",
      "userGroupMobile": null,
      "userGroupName": null,
      "userCityId": 1,
      "tokenNumber": 1102,
      "queueName": "ENT",
      "businessName": "Sahyadri Clinic",
      "statusType": "T_CREATE",
      "joinDate": "2016-08-16",
      "joinTime": "11:10:00",
      "tokenMetadata": null,
      "slotGroupName": "Morning",
      "slotGroup": "09:00:00 - 12:55:00",
      "billAmount": null,
      "refundAmount": null,
      "billingType": null,
      "billingId": null,
      "isPaid": null,
      "clientUuid": "9a6ff998-f085-4183-b2c1-067e7a6d3001",
      "enablePayments": 0,
      "forcePayment": 1
    },
    {
      "tokenQueueId": 1,
      "tokenId": 3,
      "appType": 1,
      "userId": 14,
      "userName": "demo18",
      "userEmail": null,
      "userMobile": "7805802928",
      "userGroupMobile": null,
      "userGroupName": null,
      "userCityId": 1,
      "tokenNumber": 1103,
      "queueName": "ENT",
      "businessName": "Sahyadri Clinic",
      "statusType": "T_CREATE",
      "joinDate": "2016-08-16",
      "joinTime": "11:10:00",
      "tokenMetadata": null,
      "slotGroupName": "Morning",
      "slotGroup": "09:00:00 - 12:55:00",
      "billAmount": null,
      "refundAmount": null,
      "billingType": null,
      "billingId": null,
      "isPaid": null,
      "clientUuid": "8e76d11f-45db-4780-810f-93734064c423",
      "enablePayments": 0,
      "forcePayment": 1
    }
  ]
}
```

This endpoint retrieves appointment report for business given a date range.

### HTTP Request

`GET http://api.sminq.com/v1/business/reports/appointments`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue Id.
fromDate | true | Start date range.
toDate | true | End date range.


## Add user

> Register user from the business side.

```shell
curl "http://api.sminq.com/v1/business/user"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "userId": 29,
    "userName": "test",
    "userEmail": null,
    "userMobile": "9898989898",
    "createdOn": null,
    "isBusinessOwner": 0,
    "userVerified": 1,
    "userCityId": 1,
    "countryCode": null,
    "pin": null,
    "appVersion": null,
    "canBusinessEdit": null
  }
}
```

This endpoint registers a user from the business side, no verification required here.

### HTTP Request

`POST http://api.sminq.com/v1/business/user`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
userName | true | Name for user.
userMobile | true | Unique user mobile.
userVerified | true | set to 1.
isBusinessOwner | true | set to 0.
userCityId | true | city of user.

### Error codes

Code | Description
--------- | -----------
336 |  User name cannot be empty.
337 |  User mobile cannot be empty.


## Update user

```shell
curl "http://api.sminq.com/v1/user/profile/10"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "userName": "test",
    "userEmail": null,
    "userMobile": "9898989897",
    "oauthId": null,
    "isBusinessOwner": 0,
    "userVerified": 1,
    "userCityId": 1,
    "deviceId": null,
    "pin": null,
    "countryCode": null,
    "appVersion": null,
    "userId": null
  }
}
```

This endpoint updates a user profile from business side, no verification required.

### HTTP Request

`POST http://api.sminq.com/v1/user/profile/{id}`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userName | true | Name for user.
userMobile | true | Unique user mobile.
userVerified | true | set to 1.
isBusinessOwner | true | set to 0.
userCityId | true | city of user.

### Error codes

Code | Description
--------- | -----------
336 |  User name cannot be empty.
337 |  User mobile cannot be empty.

## Search User

```shell
curl "http://api.sminq.com/v1/business/user/search"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "userId": 29,
    "userName": "Test",
    "userEmail": null,
    "userMobile": "9898989897",
    "createdOn": null,
    "isBusinessOwner": null,
    "userVerified": 1,
    "userCityId": 1,
    "countryCode": null,
    "pin": "0",
    "appVersion": null,
    "canBusinessEdit": null
  }
}
```

This endpoint retrieves a user profile given a mobile.

### HTTP Request

`GET http://api.sminq.com/v1/business/user/search`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
type | true | the type here will be a mobile search  so set this to "mobile".
value | true | enter the mobile number here.


## Add Cash payment

```shell
curl "http://api.sminq.com/v1/business/bill/create"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "billingDetailsId": 100,
    "billingId": 100,
    "tokenId": 7,
    "billingType": 0,
    "amount": 200,
    "tax": 0,
    "total": 200,
    "billName": null,
    "customerType": 1,
    "customerId": 16,
    "queueId": 1
  }
}
```

This endpoint adds a cash payment for a particular appointment.

### HTTP Request

`POST http://api.sminq.com/v1/business/bill/create`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
billingType | true | 0 for cash payment.
customerType | true | 1 for user.
customerId | true | unique ID for user.
amount | true | Amount for payment.
tokenId | false | Unique token ID.

### Error codes

Code | Description
--------- | -----------
346 |  Invalid bill amount.
360 |  Invalid billing type.
102 |  Invalid queue ID.
325 |  Invalid customer ID.
325 |  Invalid customer Type.

## Confirm cash payment

```shell
curl "POST http://api.sminq.com/v1/business/bill/confirm/100"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "billingDate": null,
    "billingStatus": null,
    "tax": 0,
    "amount": 300,
    "billingType": 0,
    "tokenId": 7,
    "customerId": 16,
    "customerType": 1,
    "queueId": 1
  }
}
```

This endpoint retrieves confirms the cash payment.

### HTTP Request

`POST http://api.sminq.com/v1/business/bill/confirm/{billingId}`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
billingType | true | 0 for cash payment.
customerType | true | 1 for user.
customerId | true | unique ID for user.
amount | true | Amount for payment.
tokenId | false | Unique token ID.

### Error codes

Code | Description
--------- | -----------
346 |  Invalid bill amount.
360 |  Invalid billing type.
102 |  Invalid queue ID.
325 |  Invalid customer ID.
325 |  Invalid customer Type.

## Update cash payment

```shell
curl "POST http://api.sminq.com/v1/business/bill/update"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "billingDate": null,
    "billingStatus": null,
    "tax": 0,
    "amount": 300,
    "billingType": 0,
    "tokenId": 7,
    "customerId": 16,
    "customerType": 1,
    "queueId": 1
  }
}
```

This endpoint retrieves confirms the cash payment.

### HTTP Request

`POST http://api.sminq.com/v1/business/bill/confirm/{billingId}`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
billingType | true | 0 for cash payment.
customerType | true | 1 for user.
customerId | true | unique ID for user.
amount | true | Amount for payment.
tokenId | true | Unique token ID.

### Error codes

Code | Description
--------- | -----------
346 |  Invalid bill amount.
360 |  Invalid billing type.
102 |  Invalid queue ID.
325 |  Invalid customer ID.
325 |  Invalid customer Type.

# User
## Login


```shell
curl "http://api.sminq.com/v1/user/login"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "otp": {
      "password": "LfbsOey9yciSpch9TjHidA==",
      "mobile": "9898989897",
      "expire": null
    },
    "verified": false
  }
}
```

This endpoint is used for user login, verification is required if PIN is not entered.

### HTTP Request

`POST http://api.sminq.com/v1/user/login`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | true | Registered user mobile.
pin | true | Registered user mobile.

### Error codes

Code | Description
--------- | -----------
341 |  Mobile number is not registered.


## Verify Login

```shell
curl "http://api.sminq.com/v1/user/login/verify"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "otp": null,
    "user": {
      "userId": 29,
      "userName": "Test",
      "userEmail": null,
      "userMobile": "9898989897",
      "createdOn": null,
      "isBusinessOwner": null,
      "userVerified": 1,
      "userCityId": 1,
      "countryCode": null,
      "pin": "0",
      "appVersion": null,
      "canBusinessEdit": null
    },
    "sessionKey": "dcmn+5mPRA8F/VToB2/Kj1uAhEaV+ljx1Rr/jsz4G/IVacTDhLmdolXB4lw10SCE7y3W1zmd0rGR9bzcvpbYFk5hqJusAmST2N1dbSFODwelnfyZC1L6MkBkB4f4RMlS",
    "verified": null
  }
}
```

This endpoint retrieves user profile information after successful login.

### HTTP Request

`http://api.sminq.com/v1/user/login/verify`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | true | Registered user mobile number.
password | true | OTP sent to registered mobile number.

### Error codes

Code | Description
--------- | -----------
341 |  Mobile number is not registered.
342 |  OTP failed.


## Signup

> Registers a new user, OTP verification is required.

```shell
curl "http://api.sminq.com/v1/user/signup"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "id": null,
    "password": "CHOXAxxmOrZfzRoOGsXNRg==",
    "mobile": "9797979797"
  }
}
```

This endpoint registers a user, and sends a verification code to the mobile entered.

### HTTP Request

`POST http://api.sminq.com/v1/user/signup`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userName | true | Name for user.
userMobile | true | Unique user mobile.
userVerified | true | set to 1.
isBusinessOwner | true | set to 0.
userCityId | true | city of user.

### Error codes

Code | Description
--------- | -----------
336 |  User name cannot be empty.
337 |  User mobile cannot be empty.
340 |  User mobile is already registered.

## Verify New user

> Registers a new user, OTP verification is required.

```shell
curl "http://api.sminq.com/v1/user/verify"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "otp": null,
    "user": {
      "userId": 30,
      "userName": "Signup",
      "userEmail": null,
      "userMobile": "9797979797",
      "createdOn": null,
      "isBusinessOwner": null,
      "userVerified": 0,
      "userCityId": 1,
      "countryCode": null,
      "pin": "0",
      "appVersion": null,
      "canBusinessEdit": null
    },
    "sessionKey": "p+WdYAo3HQa4qZGQt5sElRsta0ckvwAmuH+4FVZ9Nb7LH9Ph9WW7HtEO0OqiUyHzr/pFgk36aJ3GIf+57oPLnjvMlFYdVHOa+94pjHKQxKglYlVt+AUoylpQRuC1fTb/",
    "verified": true
  }
}
```

This endpoint registers a user, and sends a verification code to the mobile entered.

### HTTP Request

`POST http://api.sminq.com/v1/user/verify`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | true | Registered mobile.
password | true | verification code sent to mobile.

### Error codes

Code | Description
--------- | -----------
342 |  User OTP failed.
337 |  User mobile cannot be empty.
336 |  User name cannot be empty.


## Device Register

> this will register user device for push notifications


```shell
curl "http://api.sminq.com/v1/user/device/register"
  -H "Authorization: xxxxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "id": 1,
    "userId": 1,
    "deviceType": "android",
    "registrationId": "ZElwXzU1Z29JZU06QVBBOTFiRTlHQ0MtVk5tMzNyWUlYQjZ4ckU3U2xUaTdIT0RVd0oweF9DaU1QbC1JbGlEWUFKRGJvaTdUMHFWX1pRVDhZS1A4RGdmUkZOdmMxZXRNTkU0TnNDbjBISHQ1QktreS1XX2xScmpXdkpweWpyVThfRHdvZXJ1c25MQjhUck1kYWNuQktvN18=",
    "deviceEndpointArn": "YXJuOmF3czpzbnM6YXAtc291dGhlYXN0LTE6Mjg4Nzg5MzU0MjQ4OmVuZHBvaW50L0dDTS9TbWlucUJ1c2luZXNzQXBwLzJlMTYxNWM5LTBmNzUtM2I4YS05Njg1LWIxNjk0ZTAwMDliMw==",
    "deviceStatus": 1
  }
}
```

This endpoint registers a business device for push notifications.

### HTTP Request

`POST http://api.sminq.com/v1/user/device/register`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | true | Unique ID for user.
deviceType | true | values can be "android" or "ios".
registrationId | true | values returned from GCM or APNS.


### Error codes

Code | Description
--------- | -----------
101 |  Invalid user ID.
303 |  Registration ID cannot be empty.


## Device Un-Register

> unregister business device for notifications

```shell
curl "http://api.sminq.com/v1/user/device/unregister"
  -H "Authorization: xxxxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": null
}
```

### HTTP Request

`POST http://api.sminq.com/v1/user/device/unregister`

### Query Parameters

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | true | Unique ID for User.
deviceType | true | values can be "android" or "ios".
registrationId | true | values returned from GCM or APNS.


### Error codes

Code | Description
--------- | -----------
101 |  Invalid business ID.
303 |  Registration ID cannot be empty.


## Update profile

> updated the user profile, OTP verification is required of mobile is changed

```shell
curl "http://api.sminq.com/v1/user/profile/update"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this if mobile is not changed:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "mobile": "9797979797",
    "password": null,
    "optEnabled": 0
  }
}
```

> The above command returns JSON structured like this if mobile is  changed:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "mobile": "9797979798",
    "password": "WA/blE5Y58DHJ2rZL8tafg==",
    "optEnabled": 1
  }
}
```

This endpoint updates a user profile.

### HTTP Request

`POST http://api.sminq.com/v1/user/profile/update`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userName | true | User full name.
userMobile | true | User mobile.
userEmail | false | User email.
userCityId | true | User city ID.
userId | true | Unique user ID.

### Error codes

Code | Description
--------- | -----------
110 |  Invalid user ID.
336 |  User name cannot be empty.
337 | User mobile cannot be empty.
104 | invalid city ID
350 | User already exists (incase of mobile number change)


## Verify profile

```shell
curl "http://api.sminq.com/v1/user/profile/verify"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "userId": 30,
    "userName": "Signup1",
    "userEmail": "demo@sminq.com",
    "userMobile": "9797979798",
    "createdOn": null,
    "isBusinessOwner": null,
    "userVerified": null,
    "userCityId": 1,
    "countryCode": null,
    "pin": null,
    "appVersion": null,
    "canBusinessEdit": null
  }
}
```

This endpoint updates user profile after verification.

### HTTP Request

`GET http://api.sminq.com/v1/user/profile/verify`

### POST Parameters

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userName | true | User full name.
userMobile | true | User mobile.
userEmail | false | User email.
userCityId | true | User city ID.
userId | true | Unique user ID.
password | true | Verification code sent to new mobile number.

### Error codes

Code | Description
--------- | -----------
110 |  Invalid user ID.
336 |  User name cannot be empty.
337 | User mobile cannot be empty.
104 | invalid city ID
350 | User already exists (incase of mobile number change)
342 | User OTP failed.

## Create Pin

> User can create a PIN to login, to avoid OTP verification on every login.

```shell
curl "http://api.sminq.com/v1/user/pin/generate"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "mobile": "9797979798",
    "pin": "F1JhJzIT6EkTf+saemUtRQ==",
    "password": null
  }
}
```

This endpoint sets PIN for user .

### HTTP Request

`POST http://api.sminq.com/v1/user/pin/generate`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | true | Registered user mobile.
pin | true | User 4 digit pin.

### Error codes

Code | Description
--------- | -----------
337 |  User mobile cannot be empty.
341 |  User does not exist.

## Add member

```shell
curl "http://api.sminq.com/v1/user/member"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this if mobile is not entered:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "mobile": null,
    "password": null,
    "optEnabled": 0
  }
}
```

> The above command returns JSON structured like this if mobile is entered (OTP verification is required):

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "mobile": "9797979777",
    "password": "fvFB4K3ZZ1GLMU7yVMBHug==",
    "optEnabled": 1
  }
}
```

This endpoint adds a member for user, if mobile is entered then verification will be required.

### HTTP Request

`POST http://api.sminq.com/v1/user/member`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | false | member mobile.
name | true | member name.
userId | true | The user for which member needs to be added.

### Error codes

Code | Description
--------- | -----------
110 |  Invalid user ID.
353 |  Name cannot be empty.
350 | member exists (if mobile is entered).

## Get members

> fetch members for a user.

```shell
curl "http://api.sminq.com/v1/user/member"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "groupId": 3,
      "userId": 30,
      "name": "Family",
      "mobile": null,
      "status": 1,
      "verified": 1,
      "createdOn": null
    }
  ]
}
```

This endpoint retrieves all members associated with a user.

### HTTP Request

`GET http://api.sminq.com/v1/user/member`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | true | Unique user ID.

Code | Description
--------- | -----------
110 |  Invalid user ID.


## Verify member

```shell
curl "http://api.sminq.com/v1/user/member/verify"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": null
}
```

This endpoint verifies member mobile number.

### HTTP Request

`POST http://api.sminq.com/v1/user/member/verify`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | false | member mobile.
name | true | member name.
userId | true | The user for which member needs to be added.
status | true | 1
password | true | OTP sent to member mobile.

### Error codes

Code | Description
--------- | -----------
110 |  Invalid user ID.
353 |  Name cannot be empty.
355 | Mobile cannot be empty.
342 | OTP verification failed.

## Update member

```shell
curl "http://api.sminq.com/v1/user/member/5"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this if mobile is not entered:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "mobile": null,
    "password": null,
    "optEnabled": 0
  }
}
```

> The above command returns JSON structured like this if mobile is entered (OTP verification is required):

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "mobile": "9797979777",
    "password": "fvFB4K3ZZ1GLMU7yVMBHug==",
    "optEnabled": 1
  }
}
```

This endpoint update a member for user, if mobile is entered then verification will be required.

### HTTP Request

`POST http://api.sminq.com/v1/user/member/{groupId}`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | false | member mobile.
name | true | member name.
userId | true | The user for which member needs to be added.
groupId | true | The unique member ID

### Error codes

Code | Description
--------- | -----------
110 |  Invalid user ID.
353 |  Name cannot be empty.
350 | member exists (if mobile is entered).


## Verify member update

> verify the updated mamber if mobile was changed.

```shell
curl "http://api.sminq.com/v1/user/member/verify/{id}"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:


```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "groupId": 3,
      "userId": 30,
      "name": "Family",
      "mobile": 999999999,
      "status": 1,
      "verified": 1,
      "createdOn": null
    }
  ]
}
```

This endpoint updateds member after new mobile is verified.

### HTTP Request

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | false | member mobile.
name | true | member name.
userId | true | The user for which member needs to be added.
status | true | 1
password | true | OTP sent to member mobile.


### Error codes

Code | Description
--------- | -----------
110 |  Invalid user ID.
353 |  Name cannot be empty.
355 | Mobile cannot be empty.
342 | OTP verification failed.

## Business search


```shell
curl "http://api.sminq.com/v1/user/business/search"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this if mobile is not entered:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "businessId": 43,
      "businessCategoryId": 1,
      "businessName": "Aadhar Hospital",
      "businessLatitude": 18.519489464507107,
      "businessLongitude": 73.90624713897705,
      "mobile": "9822052621",
      "address": "Dr. Ravindrakumar Katkar, 36, Inna Appt, Opp. Pune Mahila Mandal, Parvati, Pune",
      "cityName": "Pune",
      "cityId": 1,
      "tags": "General Medicine",
      "openQueues": 1,
      "verticalType": "clinic",
      "enableAdvanceBooking": 1,
      "enablePayments": 0,
      "queueId": 61
    },
    {
      "businessId": 24,
      "businessCategoryId": 1,
      "businessName": "Dr Bhalerao Children Hospital",
      "businessLatitude": 18.5793178,
      "businessLongitude": 18.5793178,
      "mobile": "9850198501",
      "address": "4 - B, Narsinha Colony, Narsinha School Lane, Old Sangavi, Pune",
      "cityName": "Pune",
      "cityId": 1,
      "tags": "Pediatrician",
      "openQueues": 2,
      "verticalType": "clinic",
      "enableAdvanceBooking": 1,
      "enablePayments": 1,
      "queueId": 32
    }
  ]
}
```

This endpoint retrieves all business.

### HTTP Request

`GET http://api.sminq.com/v1/user/business/search`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cityId | true | City of user to search for business.
categoryId | true | business category.
cityName | false | name of city.
categoryName | false | name of category.
location | false | location name.
tags | false | business speciality.
limit | false | for pagination.
page | false | for pagination.



## Check availability

> check calendar availability for a business queue.

```shell
curl "http://api.sminq.com/v1/user/queue/availability"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "nextTimeSlot": "19:00:00",
    "availableSlotCount": 15,
    "joinDate": "2016-08-16",
    "groupStartTime": "18:00:00",
    "groupEndTime": "20:55:00",
    "message": "FULL NOW\nNext available at 07:00 PM\n(ONLY 15 places)"
  }
}
```

This endpoint checks calendar availability for a business queue.

### HTTP Request

`GET http://api.sminq.com/v1/user/queue/availability`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | unique business queue ID.


## Add Online payment


```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Confirm online payment


```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Verify online payment


```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Check payment status


```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>


## Get Appointments

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get active alert


```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>


## Update time slot

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

# Common

## Create appointment

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Reschedule appointment

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Change appointment status

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Add appointment details

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get appointment slots to update

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get apppointment slots

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Confirm appointment

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Schedule appointment

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Cancel scheduled appointment

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get time to schedule

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get categories

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get search tags

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get city languages

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get city locations

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get app config

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

