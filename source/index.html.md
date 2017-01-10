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
100 |  This mobile is not registered.

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
        "weeklyAvgWaitTime": 0,
        "isFavorite": 1
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

This endpoint retrieves all queues for a business. If userId is passed along in request then a flag "isFavorite" is added to the queues in response which indicates that a queue is marked 'favorite' by the user.

### HTTP Request

`GET http://api.sminq.com/v1/business/queue/search`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
businessId | true | Unique ID for business.
queueAccess | false | Permissions to restrict queue access for business.
parentQueue | false | 
userId | false | User Id


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

## Queue Monthly calendar

> Get the queue calendar for entire month:


```shell
curl "http://api.sminq.com/v1/business/queue/calendar/availability"
  -H "Authorization: xxxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "date": "2016-10-15",
      "day": "Saturday",
      "availability": 405
    },
    {
      "date": "2016-10-16",
      "day": "Sunday",
      "availability": 450
    },
    {
      "date": "2016-10-17",
      "day": "Monday",
      "availability": 450
    },
    {
      "date": "2016-10-18",
      "day": "Tuesday",
      "availability": 295
    },
    {
      "date": "2016-10-19",
      "day": "Wednesday",
      "availability": 323
    },
    {
      "date": "2016-10-20",
      "day": "Thursday",
      "availability": 289
    },
    {
      "date": "2016-10-21",
      "day": "Friday",
      "availability": 403
    },
    {
      "date": "2016-10-22",
      "day": "Saturday",
      "availability": 405
    },
    {
      "date": "2016-10-23",
      "day": "Sunday",
      "availability": 450
    },
    {
      "date": "2016-10-24",
      "day": "Monday",
      "availability": 450
    },
    {
      "date": "2016-10-25",
      "day": "Tuesday",
      "availability": 295
    },
    {
      "date": "2016-10-26",
      "day": "Wednesday",
      "availability": 323
    },
    {
      "date": "2016-10-27",
      "day": "Thursday",
      "availability": 289
    },
    {
      "date": "2016-10-28",
      "day": "Friday",
      "availability": 403
    },
    {
      "date": "2016-10-29",
      "day": "Saturday",
      "availability": 405
    },
    {
      "date": "2016-10-30",
      "day": "Sunday",
      "availability": 450
    }
  ]
}
```

This endpoint retrieves the queue calendar availability for entire month.

### HTTP Request

`GET http://api.sminq.com/v1/business/queue/calendar/availability`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique queueId .
date | true | current date.

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.

## Update time slot

> modify time slots for business queue.

```shell
curl "http://api.sminq.com/v1/business/queue/slots/update"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "startTimeSlot": "15:00:00",
    "endTimeSlot": "16:00:00",
    "tokenUpperLimit": null,
    "queueId": 1,
    "updateAction": "status",
    "slotStatus": 0,
    "date": 1471392000000
  }
}
```

This endpoint updates status or upperlimit of time slots, based on the update action.

### HTTP Request

`POST http://api.sminq.com/v1/business/queue/slots/update`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue.
startTimeSlot | true | Start time range.
endTimeSlot | true | End time range.
updateAction | true | can be "status" or "limit".
slotStatus | false | true if updateAction="status" and value can be 0 - Turn off, 1 - Turn on
tokenUpperLimit | false | true if updateAction="limit"
date | true | date for which slots need to be updated.

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.
317 | No slots available between selected time.
318 | Some slots seem to have appointments.


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
    "totalCash": 200,
    "weeklyAvgWaitTime": null,
    "totalOnline": 500,
    "totalDiscountAdjusted": 20
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
    "optEnabled": 0,
    "status": 1
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
    "optEnabled": 1,
    "status": 1
  }
}
```

This endpoint update a member for user, if mobile is entered then verification will be required.
If 'status' field is 0 then member is deactivated and will not appear in User's member list. 

### HTTP Request

`POST http://api.sminq.com/v1/user/member/{groupId}`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
mobile | false | member mobile.
name | true | member name.
userId | true | The user for which member needs to be added.
groupId | true | The unique member ID
status | true | member status. 1 = active; 0 = deleted

### Error codes

Code | Description
--------- | -----------
110 |  Invalid user ID.
353 |  Name cannot be empty.
350 | member exists (if mobile is entered).


## Verify member update

> verify the updated member if mobile was changed.

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


## Mark Favorite

> Mark/Unmark a queue as favorite

```shell
curl "http://api.sminq.com/v1/user/favorite"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "userId": 3,
    "queueId": 1,
    "businessId": 1
  }
}
```
This endpoint marks or unmarks a Queue as favorite.

### HTTP Request

`POST http://api.sminq.com/v1/user/favorite`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Business queue to mark favorite.
userId | true | User who is marking queue as favorite.
businessId | true | Unique Business id where queue belongs
status | true | 1: Mark Favorite; 0: Unmark Favorite


## Get Favorites

```shell
curl "http://api.sminq.com/v1/user/favorite"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "businessId": 1,
      "businessName": "Health Clinic",
      "businessCategoryId": 1,
      "businessCityId": 1,
      "cityName": "Pune",
      "address": "Regus Business Center, Sky One, Kalyani Nagar",
      "email": "keyan@sminq.com",
      "mobile": "9158421603",
      "queueId": 1,
      "queueName": "ENT",
      "businessLatitude": "18.5049",
      "businessLongitude": "73.9005996"
    },
    {
      "businessId": 2,
      "businessName": "Pets Clinic",
      "businessCategoryId": 1,
      "businessCityId": 1,
      "cityName": "Pune",
      "address": "Superior General : Sr. Santan Nago, Fatima Convent, Fatima Nagar\nParmar Nagar, Wanwadi",
      "email": "sheldond@gmail.com",
      "mobile": "9890005358",
      "queueId": 2,
      "queueName": "Pets",
      "businessLatitude": "18.5049",
      "businessLongitude": "73.9005996"
    }
  ]
}
```

This endpoint retrieves all favorite businesses.

### HTTP Request

`GET http://api.sminq.com/v1/user/favorite`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | false | User id to retreive favorites for

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
      "queueId": 61,
      "availability": "ONLY 14 places availabletill 09:00 AM",
      "sminqCertified": 1
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
      "queueId": 32,
      "availability": "ONLY 14 places availabletill 09:00 AM",
      "sminqCertified": 0
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
latitude | false | for geo sorting
longitude | false | for geo sorting

## Autocomplete search


```shell
curl "http://api.sminq.com/v1/user/business/autocomplete"
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

This endpoint retrieves all business matching text entered.

### HTTP Request

`GET http://api.sminq.com/v1/user/business/autocomplete`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cityId | true | City of user to search for business.
categoryId | true | business category.
searchText | true | text entered for search
limit | false | for pagination.
page | false | for pagination.

## Get Queue Profile

```shell
curl "http://api.sminq.com/v1/queue/profile"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "queueName": "Dr. Ganesh Morya",
    "businessName": "Ganesh Test Clinic",
    "mobile": "1234567890",
    "email": "niket@sminq.com",
    "address": "DP Road, Koregaon Park, Pune 411001",
    "businessLatitude": "18.519489464507107",
    "businessLongitude": "73.90624713897705",
    "profileImage": null,
    "tags": "Dermatologist, General Practice, Gynecologist, Orthopedic, Pediatrician, Demo, Test, Ayurvedic, General Medicine, ENT, Physiotherapists, General Surgeon, Ophthalmologist, Neurologist, Diabetologist, Cardiologists, Dentist",
    "timings": "MO=06:00 - 12:45|13:00 - 17:45|18:00 - 23:30,TU=08:00 - 14:00|14:15 - 17:45|18:00 - 22:00,WE=08:00 - 14:00|14:15 - 17:45|18:00 - 22:00,TH=08:00 - 13:00|18:00 - 22:00,FR=08:00 - 13:00|18:00 - 22:00,SA=08:00 - 13:00|18:00 - 22:00,",
    "metadata": "Allrounder Doctor ",
    "advanceBookingDays": 7,
    "enableAdvanceBookingForToday": 0,
    "enableAdvanceBookings": 1,
    "isMonetizationApplicable": 1,
    "liveAlerts": [
      {
        "id": 6,
        "queueId": 1,
        "message": "New patients call at clinic to take appointment.",
        "liveDate": "2016-10-10",
        "endDate": "2016-10-10",
        "startTime": null,
        "endTime": null,
        "status": 1,
        "stopQueue": 0,
        "allDay": 1,
        "sendToExisting": null
      }
    ]
  }
}
```

This endpoint retrieves profile of a Queue.

### HTTP Request

`GET http://api.sminq.com/v1/queue/profile`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | false | Queue ID for profile look up

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.

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


## Get Appointments

> get live and upcoming appointments for a user.

```shell
curl "http://api.sminq.com/v1/user/appointments"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "liveTokens": [
      {
        "tokenQueueId": 1,
        "tokenId": 30,
        "appType": 1,
        "userId": 5,
        "userName": "demo8",
        "userEmail": null,
        "userMobile": "4334048553",
        "userGroupMobile": null,
        "userGroupName": null,
        "userCityId": 1,
        "tokenNumber": 1105,
        "queueName": "ENT",
        "businessName": "Sahyadri Clinic",
        "statusType": "T_CREATE",
        "joinDate": "2016-08-17",
        "joinTime": "11:30:00",
        "tokenMetadata": null,
        "slotGroupName": "Morning",
        "slotGroup": "09:00:00 - 12:55:00",
        "billAmount": null,
        "refundAmount": null,
        "billingType": null,
        "billingId": null,
        "isPaid": null,
        "clientUuid": "ba3a46d9-4577-48eb-80ba-d30dc09913b7",
        "enablePayments": 0,
        "forcePayment": 1,
        "tags": "General Practice",
        "cityName": "Pune",
        "verticalType": "Clinic",
        "sminqCertified": 0,
        "isUserMembershipValid": 0,
        "isMonetizationApplicable": 1,
        "interimAmount": null
      }
    ],
    "upcomingTokens": [
      {
        "tokenQueueId": 1,
        "tokenId": 32,
        "appType": 1,
        "userId": 5,
        "userName": "demo8",
        "userEmail": null,
        "userMobile": "4334048553",
        "userGroupMobile": null,
        "userGroupName": null,
        "userCityId": 1,
        "tokenNumber": 1201,
        "queueName": "ENT",
        "businessName": "Sahyadri Clinic",
        "statusType": "T_CREATE",
        "joinDate": "2016-08-18",
        "joinTime": "12:30:00",
        "tokenMetadata": null,
        "slotGroupName": "Morning",
        "slotGroup": "09:00:00 - 12:55:00",
        "billAmount": null,
        "refundAmount": null,
        "billingType": null,
        "billingId": null,
        "isPaid": null,
        "clientUuid": "f81f784f-ea54-4677-93f6-6667c0f35933",
        "enablePayments": 0,
        "forcePayment": 1,
        "tags": "General Practice",
        "cityName": "Pune",
        "verticalType": "Clinic",
        "sminqCertified": 0,
        "isUserMembershipValid": 0,
        "isMonetizationApplicable": 1,
        "interimAmount": null
      }
    ]
  }
}
```

This endpoint retrieves live and upcoming tokens for a user..

### HTTP Request

`GET http://api.sminq.com/v1/user/appointments`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | true | Registered user ID.
limit | false | for pagination.
page | false | for pagination.

## Get Appointment Status

> get live status of appointment.

```shell
curl "http://api.sminq.com/v1/user/appointment/status"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "businessName": "Sahyadri Clinic",
    "businessId": 1,
    "queueName": "ENT",
    "tokenNumber": 1910,
    "tokenId": 10,
    "joinDate": "2016-08-26",
    "joinTime": "19:00:00",
    "avgServingTime": 10,
    "verticalType": "clinic",
    "lastCompleted": "18:37:05",
    "createdOn": "2016-08-26 18:36:58",
    "queueId": 1,
    "peopleAheadOfYou": 8,
    "statusType": "T_CREATE",
    "enableAdvanceBooking": 1,
    "enableAdvanceBookingForToday": 0,
    "advanceBookingDays": 7,
    "enablePayments": 0,
    "changeInPeopleAhead": 0,
    "forcePayment": 1
  }
}
```

This endpoint retrieves live status for a user appointment..

### HTTP Request

`GET http://api.sminq.com/v1/user/appointment/status`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
tokenId | true | Registered appointment ID.
queueId | true | Unique business queue ID.

## Get business alert

> the the active alert set by business, to notify user.

```shell
curl "http://api.sminq.com/user/alert"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "id": null,
    "queueId": null,
    "message": "Dr will not be available today, you can take appointments with Assistant",
    "liveDate": "2016-08-17",
    "endDate": "2016-08-17",
    "startTime": null,
    "endTime": null,
    "status": null,
    "stopQueue": 0,
    "allDay": 1,
    "sendToExisting": null
  }
}
```

This endpoint retrieves the live alert set by business.

### HTTP Request

`GET http://api.sminq.com/user/alert`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queueID.

## Get business alert

> the the active alert set by business, to notify user.

```shell
curl "http://api.sminq.com/user/alert"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "id": null,
    "queueId": null,
    "message": "Dr will not be available today, you can take appointments with Assistant",
    "liveDate": "2016-08-17",
    "endDate": "2016-08-17",
    "startTime": null,
    "endTime": null,
    "status": null,
    "stopQueue": 0,
    "allDay": 1,
    "sendToExisting": null
  }
}
```

This endpoint retrieves the live alert set by business.

### HTTP Request

`GET http://api.sminq.com/user/alert`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queueID.

## User Preference

> Add user preference e.g notification language preference.

```shell
curl "http://api.sminq.com/v1/user/preference"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "preferenceId": 1,
    "userId": 1,
    "languageId": 2,
    "marketingOptIn": null,
  }
}
```

This endpoint sets user preference.

### HTTP Request

`POST http://api.sminq.com/v1/user/preference`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | true | Registered user ID.
languageId | true | Language preference.
marketingOptIn | false | opt in for marketing messages.

## Get User Preference

> Get user preference .

```shell
curl "http://api.sminq.com/v1/user/preference"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "preferenceId": 1,
    "userId": 1,
    "languageId": 2,
    "marketingOptIn": null,
  }
}
```

This endpoint retrieves user preference set in system.

### HTTP Request

`GET http://api.sminq.com/v1/user/preference`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | true | Registered user ID.

## User Suggestion

> Capture user business suggestion.

```shell
curl "http://api.sminq.com/v1/user/preference/suggest"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "userId": 1,
    "userName": "",
    "userEmail": "",
    "userMobile": "",
  }
}
```

This endpoint captures user business lead suggestion.

### HTTP Request

`POST http://api.sminq.com/v1/user/preference/suggest`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | true | Registered user ID.
info | true | info about the business.
contact | true | suggested business contact.

# Common

## Create appointment

```shell
curl PUT "http://api.sminq.com/v1/appointment/create"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "tokenId": 33,
    "tokenQueueId": 1,
    "totalProcessTime": null,
    "tokenUser": 31,
    "joinDate": "2016-08-17",
    "joinTime": "18:30:00",
    "appType": null,
    "isAdvance": null,
    "isConfirmed": null,
    "tokenUserGroup": null,
    "uuid": null,
    "tokenNumber": 1801
  }
}
```

This endpoint creates an appointment for business.

### HTTP Request

`PUT http://api.sminq.com/v1/appointment/create`

### PUT Parameters

Parameter | Default | Description
--------- | ------- | -----------
tokenQueueId | true | Business queueId for appointment.
userName | true | Name of the user.
userMobile | true | User contact.
joinTime | false | Time for appointment, if not set the system selects next available time.
joinDate | true | Date for appointment.
appType | true | appointment created from which channel 
businessApp | false | If set to 1 the appointment is added from business side.
tokenUserId | false |  if user is logged in send across id.
notes | false | Any notes to be added while taking appointment.

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.
326 |  Business blocked appointments.
322 |  Invalid join date.
323 |  Invalid join time.
324 |  Advance bookings not accepted.
325 |  Invalid user.

## Reschedule appointment

> Use this api to reschedule existing appointment.

```shell
curl "http://api.sminq.com/v1/appointment/reschedule"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "tokenId": 33,
    "queueId": 1,
    "joinDate": 1471392000000,
    "joinTime": "19:00:00",
    "appType": 1,
    "batch": false
  }
}
```

This endpoint reschedules an appointment to another date and time. This api call is asynchronous.

### HTTP Request

`POST http://api.sminq.com/v1/appointment/reschedule`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Business queue for appointment.
tokenId | true | Existing appointment ID.
joinDate | true | new join date
joinTime | true | new join time
appType | true | channel for reschedule.

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.
103 |  Invalid token ID.
108 |  Invalid join date/time.
  

## Change appointment status

> change status for appointment.

```shell
curl "http://api.sminq.com/v1/appointment/status"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "statusId": null,
      "tokenId": 34,
      "tokenStatusId": 3,
      "changedAt": 1471413042267,
      "changedOn": 1471412675000,
      "createdOn": 1471412675000
    }
  ]
}
```

This endpoint changes staus for an appointment.

### HTTP Request

`PUT http://api.sminq.com/v1/appointment/status`

### PUT Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business ID.
tokenId | true | Unique appointment id.
statusType | true | Status to be set 

### Valid status types

Type | Description
--------- | -----------
T_PROCESS | process a particular appointment
T_ABSENT  | mark a particular appoint as absent (user not yet arrived).
T_PRESENT | mark appoinment as present (User has arrived).
T_CANCEL  | cancel an appointment.
T_CLOSED  | appointment was successfully completed. 

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue ID.
103 |  Invalid token ID.
109 |  StatusType cannot be null or empty.
332 |  Token cannot be marked absent
328 |  This token status is not allowed.
329 |  Token status can no longer be changed.
330 |  Invalid token status

## Add appointment details

> Add some metadata for appointment

```shell
curl "http://api.sminq.com/v1/appointment/details"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "tokenId": 34,
    "comment": "Some user details regarding appointment.",
    "uuid": null
  }
}
```

This endpoint sets details for appointment, reason fro appointment.

### HTTP Request

`POST http://api.sminq.com/v1/appointment/details`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
tokenId | true | unique appointment ID.
comment | true | details for the appointment.

### Error codes

Code | Description
--------- | -----------
103 |  Invalid token ID.
335 |  Comment cannot be empty.

## Get appointment slots to update

> fetch available slot for business to update

```shell
curl "http://api.sminq.com/v1/appointment/slots/select"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "slotId": 1953,
      "startTimeSlot": "11:30:00",
      "endTimeSlot": "12:00:00",
      "status": 1,
      "tokenUpperLimit": 15
    },
    {
      "slotId": 1954,
      "startTimeSlot": "12:00:00",
      "endTimeSlot": "12:30:00",
      "status": 1,
      "tokenUpperLimit": 15
    },
    {
      "slotId": 1955,
      "startTimeSlot": "12:30:00",
      "endTimeSlot": "13:00:00",
      "status": 1,
      "tokenUpperLimit": 15
    },
    {
      "slotId": 1956,
      "startTimeSlot": "13:00:00",
      "endTimeSlot": "13:30:00",
      "status": 0,
      "tokenUpperLimit": 15
    }
  ]
}
```

This endpoint retrieves all slots for a business for a given date.

### HTTP Request

`GET http://api.sminq.com/v1/appointment/slots/select`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | business for which slots have to retrieved.
date | true | slots for which date.


## Get appointment slots

> get all slots available for taking an appointment.

```shell
curl "http://api.sminq.com/v1/appointment/slots"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "slotId": 1953,
      "queueId": 1,
      "startTimeSlot": "11:30:00",
      "endTimeSlot": "12:00:00",
      "tokenUpperLimit": null,
      "day": "Wed",
      "sequence": 16,
      "status": null,
      "groupId": 16,
      "openToUser": 1,
      "premiumSlot": 0,
      "usedSlots": 1,
      "totalSlots": 15,
      "groupStartTime": "08:00:00",
      "groupEndTime": "14:00:00"
    },
    {
      "slotId": 1954,
      "queueId": 1,
      "startTimeSlot": "12:00:00",
      "endTimeSlot": "12:30:00",
      "tokenUpperLimit": null,
      "day": "Wed",
      "sequence": 17,
      "status": null,
      "groupId": 16,
      "openToUser": 1,
      "premiumSlot": 0,
      "usedSlots": 0,
      "totalSlots": 15,
      "groupStartTime": "08:00:00",
      "groupEndTime": "14:00:00"
    },
    {
      "slotId": 1955,
      "queueId": 1,
      "startTimeSlot": "12:30:00",
      "endTimeSlot": "13:00:00",
      "tokenUpperLimit": null,
      "day": "Wed",
      "sequence": 18,
      "status": null,
      "groupId": 16,
      "openToUser": 1,
      "premiumSlot": 0,
      "usedSlots": 0,
      "totalSlots": 15,
      "groupStartTime": "08:00:00",
      "groupEndTime": "14:00:00"
    }
  ]
}
```

This endpoint retrieves all slots available for taking an appointment, if usedSlots == totalSlots then slot is full.

### HTTP Request

`GET http://api.sminq.com/v1/appointment/slots`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | business for which slots have to retrieved.
date | true | slots for which date.


## Get appointment groups

> get all slots under each available group for taking an appointment.

```shell
curl "http://api.sminq.com/v1/appointment/groups"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "groupName": "Evening",
      "groupId": 25,
      "groupStartTime": "18:00:00",
      "groupEndTime": "20:55:00",
      "slots": [
        {
          "slotId": 41800,
          "queueId": 1,
          "startTimeSlot": "18:00:00",
          "endTimeSlot": "18:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 25,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "18:00:00",
          "groupEndTime": "20:55:00",
          "availableJoinDate": null,
          "groupName": "Evening"
        },
        {
          "slotId": 41830,
          "queueId": 1,
          "startTimeSlot": "18:30:00",
          "endTimeSlot": "19:00:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 25,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "18:00:00",
          "groupEndTime": "20:55:00",
          "availableJoinDate": null,
          "groupName": "Evening"
        },
        {
          "slotId": 41900,
          "queueId": 1,
          "startTimeSlot": "19:00:00",
          "endTimeSlot": "19:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 25,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "18:00:00",
          "groupEndTime": "20:55:00",
          "availableJoinDate": null,
          "groupName": "Evening"
        },
        {
          "slotId": 41930,
          "queueId": 1,
          "startTimeSlot": "19:30:00",
          "endTimeSlot": "20:00:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 25,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "18:00:00",
          "groupEndTime": "20:55:00",
          "availableJoinDate": null,
          "groupName": "Evening"
        },
        {
          "slotId": 42000,
          "queueId": 1,
          "startTimeSlot": "20:00:00",
          "endTimeSlot": "20:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 25,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "18:00:00",
          "groupEndTime": "20:55:00",
          "availableJoinDate": null,
          "groupName": "Evening"
        },
        {
          "slotId": 42030,
          "queueId": 1,
          "startTimeSlot": "20:30:00",
          "endTimeSlot": "21:00:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 25,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "18:00:00",
          "groupEndTime": "20:55:00",
          "availableJoinDate": null,
          "groupName": "Evening"
        }
      ]
    },
    {
      "groupName": "Morning",
      "groupId": 11,
      "groupStartTime": "09:00:00",
      "groupEndTime": "12:55:00",
      "slots": [
        {
          "slotId": 40900,
          "queueId": 1,
          "startTimeSlot": "09:00:00",
          "endTimeSlot": "09:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 11,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "09:00:00",
          "groupEndTime": "12:55:00",
          "availableJoinDate": null,
          "groupName": "Morning"
        },
        {
          "slotId": 40930,
          "queueId": 1,
          "startTimeSlot": "09:30:00",
          "endTimeSlot": "10:00:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 11,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "09:00:00",
          "groupEndTime": "12:55:00",
          "availableJoinDate": null,
          "groupName": "Morning"
        },
        {
          "slotId": 41000,
          "queueId": 1,
          "startTimeSlot": "10:00:00",
          "endTimeSlot": "10:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 11,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "09:00:00",
          "groupEndTime": "12:55:00",
          "availableJoinDate": null,
          "groupName": "Morning"
        },
        {
          "slotId": 41030,
          "queueId": 1,
          "startTimeSlot": "10:30:00",
          "endTimeSlot": "11:00:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 11,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "09:00:00",
          "groupEndTime": "12:55:00",
          "availableJoinDate": null,
          "groupName": "Morning"
        },
        {
          "slotId": 41100,
          "queueId": 1,
          "startTimeSlot": "11:00:00",
          "endTimeSlot": "11:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 11,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 1,
          "totalSlots": 5,
          "groupStartTime": "09:00:00",
          "groupEndTime": "12:55:00",
          "availableJoinDate": null,
          "groupName": "Morning"
        },
        {
          "slotId": 41130,
          "queueId": 1,
          "startTimeSlot": "11:30:00",
          "endTimeSlot": "12:00:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 11,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "09:00:00",
          "groupEndTime": "12:55:00",
          "availableJoinDate": null,
          "groupName": "Morning"
        },
        {
          "slotId": 41200,
          "queueId": 1,
          "startTimeSlot": "12:00:00",
          "endTimeSlot": "12:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 11,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "09:00:00",
          "groupEndTime": "12:55:00",
          "availableJoinDate": null,
          "groupName": "Morning"
        },
        {
          "slotId": 41230,
          "queueId": 1,
          "startTimeSlot": "12:30:00",
          "endTimeSlot": "13:00:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 11,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 5,
          "groupStartTime": "09:00:00",
          "groupEndTime": "12:55:00",
          "availableJoinDate": null,
          "groupName": "Morning"
        }
      ]
    }
  ]
}
```

This endpoint retrieves all groups available for taking an appointment. It returns group information along with a list of slots that belong to that group. Availability information is associated with each slot within the group.

### HTTP Request

`GET http://api.sminq.com/v1/appointment/groups`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | business for which groups have to retrieved.
date | true | slots for which date.

## Get appointment details

> get all token details

```shell
curl "http://api.sminq.com/v1/appointment/search"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "businessName": "Shreenath Clinic",
    "businessId": 1,
    "queueName": "Dr. S S Ingle",
    "tokenNumber": 501,
    "tokenId": 197,
    "joinDate": "2016-11-10",
    "joinTime": "11:00:00",
    "avgServingTime": 10,
    "verticalType": "clinic",
    "lastCompleted": null,
    "createdOn": "2016-11-09 12:26:30",
    "queueId": 1,
    "peopleAheadOfYou": 0,
    "statusType": "T_CREATE",
    "queueStatus": null,
    "message": null,
    "stopQueue": null,
    "enableAdvanceBooking": 1,
    "enableAdvanceBookingForToday": 0,
    "advanceBookingDays": 7,
    "enablePayments": 1,
    "billAmount": null,
    "changeInPeopleAhead": 0,
    "billingType": null,
    "forcePayment": 1,
    "isPaid": null,
    "tokenMetadata": null,
    "userName": "demo9",
    "userGroupName": null,
    "userId": 4,
    "groupId": null,
    "queueProfile": {
      "queueName": "Dr. S S Ingle",
      "businessName": "Shreenath Clinic",
      "mobile": "8087592468",
      "email": null,
      "address": "Solace Park, B T Kavade Road, Gorpadi",
      "businessLatitude": 18.529252,
      "businessLongitude": 73.90995699999996,
      "profileImage": "http://s3-ap-southeast-1.amazonaws.com/sminq.in/images/staging/queue-1-1477027898231-Dr Ajit Kadam.jpeg",
      "tags": "OPD, IPD, Labs, Sonography, X-Ray, ECG, Operation Theater ",
      "timings": "MO=06:00 - 06:45|08:00 - 14:00|14:00 - 18:00|18:05 - 23:55,TU=00:15 - 05:30|08:00 - 14:00|14:00 - 18:00|18:05 - 23:55,WE=08:00 - 14:00|14:00 - 18:00|18:05 - 23:55,TH=08:00 - 14:00|14:00 - 18:00|18:05 - 23:55,FR=08:00 - 14:00|14:05 - 17:55|18:00 - 23:55,SA=08:00 - 14:00|14:00 - 18:00|18:05 - 23:55,SU=08:00 - 14:00|14:00 - 18:00|18:05 - 23:55,",
      "metadata": null,
      "enableAdvanceBooking": 1,
      "advanceBookingDays": 7,
      "enableAdvanceBookingForToday": 0,
      "liveAlerts": [],
      "forcePayment": 0,
      "enablePayments": 1,
      "queuingSystem": 0
    },
    "billingSummary": [
      {
        "billingId": 22,
        "billingDate": "2016-11-09",
        "tokenId": 197,
        "billingType": 0,
        "amount": 200,
        "tax": 0,
        "total": 200,
        "billName": null,
        "queueId": 1,
        "queueName": "Dr. S S Ingle",
        "invoiceId": 18,
        "invoiceDate": "2016-11-09",
        "isPaid": 1,
        "paymentMode": null,
        "paymentStatus": null,
        "paymentDate": null,
        "paymentGateway": null,
        "invoiceLink": null
      }
    ]
  }
```

This endpoint retrieves token details for a given token id. 

### HTTP Request

`GET http://api.sminq.com/v1/appointment/search`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | queue for which token details have to retrieved.
tokenId | true | token id for which details have to be retrieved.

## Confirm appointment

> user input to confirm an appointment.

```shell
curl "http://api.sminq.com/v1/appointment/confirmation"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "tokenId": 27,
    "statusType": "T_CANCEL",
    "queueId": 1
  }
}
```

This endpoint retrieves allows the user to confirm an appointment after reminder is fired, user can choose to confirm or cancel. This api call is asynchronous.

### HTTP Request

`GET http://api.sminq.com/v1/appointment/confirmation`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
tokenId | true | Unique business appointment id.
queueId | true | Unique business queue id.
statusType | true | T_CANCEL/T_PRESENT


## Get categories

> return all business categories

```shell
curl "http://api.sminq.com/v1/business/category"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "categoryId": 1,
      "categoryName": "Clinic",
      "categoryIconUrl": null,
      "categoryStatus": 1,
      "verticalType": "clinic",
      "cityId": 1
    }
  ]
}
```

This endpoint retrieves all business category types e.g clinic/salon.

### HTTP Request

`GET http://api.sminq.com/v1/business/category`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cityName | true | Name of the city .



## Get search tags

> get all search tags for a particular business.

```shell
curl "http://api.sminq.com/v1/business/category/tags"
  -H "Authorization: xxxxxx, X-Vertical-Type: clinic"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "tagId": 1,
      "tagName": "Dermatologist"
    },
    {
      "tagId": 2,
      "tagName": "General Practice"
    },
    {
      "tagId": 5,
      "tagName": "Pediatrician"
    },
    {
      "tagId": 4,
      "tagName": "Orthopedic"
    },
    {
      "tagId": 3,
      "tagName": "Gynecologist"
    },
    {
      "tagId": 6,
      "tagName": "Demo"
    },
    {
      "tagId": 9,
      "tagName": "Ayurvedic"
    },
    {
      "tagId": 10,
      "tagName": "General Medicine"
    },
    {
      "tagId": 11,
      "tagName": "ENT"
    },
    {
      "tagId": 12,
      "tagName": "Physiotherapists"
    },
    {
      "tagId": 13,
      "tagName": "General Surgeon"
    },
    {
      "tagId": 14,
      "tagName": "Ophthalmologist"
    },
    {
      "tagId": 15,
      "tagName": "Neurologist"
    },
    {
      "tagId": 16,
      "tagName": "Diabetologist"
    }
  ]
}
```

This endpoint retrieves all business search tags for a particular category.

### HTTP Request

`GET http://api.sminq.com/v1/business/category/tags`


## Get city languages

> retrieve all languages available for a city.

```shell
curl "http://api.sminq.com/v1/city/languages"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "cityId": 1,
      "languageId": 1,
      "isDefault": 1,
      "languageName": "English"
    },
    {
      "cityId": 1,
      "languageId": 2,
      "isDefault": 0,
      "languageName": "मराठी"
    }
  ]
}
```

This endpoint retrieves all languages applicable to a city.

### HTTP Request

`GET http://api.sminq.com/v1/city/languages`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cityId | true | Unique city id.


## Get city locations

> return all active city locations.

```shell
curl "http://api.sminq.com/v1/city/location"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "locationId": 7,
      "locationName": "Aundh",
      "locationLat": 18.557872772216797,
      "locationLong": 73.80846405029297,
      "cityId": null
    },
    {
      "locationId": 10,
      "locationName": "Bhosari",
      "locationLat": 18.63852882385254,
      "locationLong": 73.84778594970703,
      "cityId": null
    }
  ]
}
```

This endpoint retrieves all active locations set for a city.

### HTTP Request

`GET http://api.sminq.com/v1/city/location`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cityName | true | name of the city.


## Get app config

> Used for force upgrades to user/business apps

```shell
curl "http://api.sminq.com/v1/app/config"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "environment": "userapp",
    "apiVersion": "1.6.4",
    "version": "1.0.47",
    "forceUpdate": 1,
    "forceLogout": 0,
    "device": "android",
    "storeUpdateDate": "2016-08-10"
  }
}
```

This endpoint retrieves app config, if forceUpdate=1 then user/business needs to update apps.

### HTTP Request

`GET http://api.sminq.com/v1/app/config`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
env | true | valid environments "userapp" and "businessapp".
version | true | version for app upgrade e.g 1.0.46
device | true | valid devices "android" and "ios"


#Payments

## Add Interim Bill

```shell
curl "http://api.sminq.com/v1/business/bill/interim"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "interimId": 1,
    "amount": 100,
    "tokenId": 7,
  }
}
```

This endpoint adds an interim billing amount, to be used for cash or online payment.

### HTTP Request

`POST http://api.sminq.com/v1/business/bill/interim`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
tokenId | true | Unique token ID.
amount | true | Amount to be billed to customer.


### Error codes

Code | Description
--------- | -----------
346 |  Invalid bill amount.
103 |  Invalid token ID.

## Get Interim Bill

```shell
curl "http://api.sminq.com/v1/business/bill/interim"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "interimId": 1,
    "amount": 100,
    "tokenId": 7,
  }
}
```

This endpoint adds an interim billing amount, to be used for cash or online payment.

### HTTP Request

`GET http://api.sminq.com/v1/business/bill/interim`

### GET Parameters

Parameter | Default | Description
--------- | ------- | -----------
tokenId | true | Unique token ID.


### Error codes

Code | Description
--------- | -----------
103 |  Invalid token ID.


## Add Split Cash payment

```shell
curl "http://api.sminq.com/v1/split/payment/cash"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "invoices": null,
    "token": {
      "tokenId": 276,
      "tokenQueueId": 1,
      "totalProcessTime": null,
      "tokenUser": 191,
      "createdOn": null,
      "joinDate": null,
      "joinTime": null,
      "appType": null,
      "isAdvance": null,
      "isConfirmed": null,
      "tokenUserGroup": null,
      "uuid": null,
      "tokenNumber": null
    }
  }
}
```

This endpoint adds a cash payment for a particular appointment for both the billing heads passed on in charges. There is no requirement to call a "confirm" API after this call. 

### HTTP Request

`POST http://api.sminq.com/v1/split/payment/cash`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
billingType | true | 0 for cash payment.
customerType | true | 1 for user.
customerId | true | unique ID for user.
countryId | true | Country Id of the business.
tokenId | false | Unique token ID.
charges | true | (Type: Array Object) Payment details

Charges need to have:

Parameter | Default | Description
--------- | ------- | -----------
amount | true | Amount for payment.
billingHead | true | Type of payment ("subscription" or "business").
discount | false | Discount amount (if applied)

### Error codes

Code | Description
--------- | -----------
346 |  Invalid bill amount.
360 |  Invalid billing type.
102 |  Invalid queue ID.
325 |  Invalid customer ID.
325 |  Invalid customer Type.


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

## Add Online payment


```shell
curl "http://api.sminq.com/v1/user/payment/create"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "httpCode": 200,
    "status": {
        "paymentAmount": {
            "queueServices": [
                {
                    "serviceName": "Consultation",
                    "serviceCost": 500
                },
                {
                    "serviceName": "Internet handling fees",
                    "serviceCost": 100
                },
                {
                    "serviceName": "test service",
                    "serviceCost": 0
                }
            ],
            "total": 600,
            "tax": 0,
            "grandTotal": 600,
            "enterAmountFlag": 0
        },
        "billingDetails": {
            "billingDetailsId": null,
            "billingId": 1504,
            "tokenId": 7605,
            "billingType": 1,
            "amount": 600,
            "tax": null,
            "total": 600,
            "billName": null,
            "customerType": 1,
            "customerId": 87,
            "queueId": null
        },
        "paymentModes": [
            {
                "paymentModeId": 2,
                "paymentModeName": "Net Banking/Credit/Debit Card",
                "paymentModeIcon": null,
                "paymentModeCode": "NB",
                "gatewayName": "razorpay"
            },
            {
                "paymentModeId": 6,
                "paymentModeName": "Paytm Wallet",
                "paymentModeIcon": null,
                "paymentModeCode": "PAYTM",
                "gatewayName": "paytm"
            }
        ]
    }
}
```

This endpoint creates a payment record, and sends across payment modes.

### HTTP Request

`POST http://api.sminq.com/v1/user/payment/create`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue Id.
tokenId | false | Unique token id, in case of pre-payment this id generated later.
customerId | true | customer unique id.
customerType | true | customer type 1 - User 2 - business.
countryId | true | required to fetch payment modes per country.

### Error codes

Code | Description
--------- | -----------
102 |  Invalid queue Id.
105 |  Invalid country id.
110 |  Invalid user ID.
359 |  Invalid customer Type.


## Confirm online payment

> confirm payment after response returned from payment gateway.

```shell
curl "http://api.sminq.com/v1/user/payment/confirm"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "httpCode": 200,
    "status": {
        "invoice": null,
        "token": {
            "tokenId": 7605,
            "tokenQueueId": 1,
            "totalProcessTime": null,
            "tokenUser": null,
            "joinDate": "2016-08-17",
            "joinTime": "11:30:00",
            "appType": null,
            "isAdvance": null,
            "isConfirmed": null,
            "tokenUserGroup": null,
            "uuid": null,
            "tokenNumber": 1102
        }
    }
}
```

This endpoint confirms payment with status after response is returned from payment gateway.

### HTTP Request

`POST http://api.sminq.com/v1/user/payment/confirm`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
paymentOrderId | true | The appointment id for which payment is done.
paymentBillingId | true | The payment billing reference id.
paymentMode | true | payment mode for appointment.
paymentGatewayId | true | the gateway transaction id.
paymentStatus | true | the payment status returned by gateway.
paymentAmount | true | the amount payment was made for
paymentQueueId | true | the business queue accepting payments.
paymentBank | false | bank for payment
paymentGateway | true | the payment gateway used for payment.

Code | Description
--------- | -----------
360 |  Invalid billing id.
361 |  Invalid payment mode.
362 |  Invalid payment amount.


## Verify online payment

> verify the amount for online payment.

```shell
curl "http://api.sminq.com/v1/user/payment/verify"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "httpCode": 200,
    "status": {
        "verifiedAmount": 600,
        "billingId": 1504,
        "gatewayOrderId": "1504"
    }
}
```

This endpoint verifies the payment amount before sending to gateway. Gateway order received by clients need to be sent back to the server in payment/confirm API call. 

### HTTP Request

`GET http://api.sminq.com/v1/user/payment/verify`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue Id.
tokenId | false | Unique token id, in case of pre-payment this id generated later.
customerId | true | customer unique id.
customerType | true | customer type 1 - User 2 - business.
billingId | true | unique invoice id.
paymentMode | true | payment mode id selected in 'byte'.
amount | true | amount to be verified.

### Error codes

Code | Description
--------- | -----------
362 |  Invalid amount.

## Get split service charges

```shell
curl "http://api.sminq.com/v1/split/payment/charges"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "queueServices": [],
    "enterAmountFlag": 1
    "paymentModes": [
      {
        "paymentModeName": "Net Banking/Credit/Debit Card",
        "paymentModeIcon": null,
        "paymentModeCode": "NB",
        "paymentModeId": "2",
        "gatewayName": "razorpay"
      },
      {
        "paymentModeName": "Paytm Wallet",
        "paymentModeIcon": null,
        "paymentModeCode": "PAYTM",
        "paymentModeId": "6",
        "gatewayName": "paytm"
      }
    ]
  }
}
```

This endpoint is used to get the business service charges if any.

### HTTP Request

`GET http://api.sminq.com/v1/split/payment/charges`

### GET Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
countryId | true | Unique country ID of the business.


## Add Split Online payment

```shell
curl "http://api.sminq.com/v1/split/payment/create"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "paymentAmount": [
      {
        "queueServices": [],
        "total": 100,
        "tax": 0,
        "grandTotal": 100,
        "enterAmountFlag": 1
      },
      {
        "queueServices": [],
        "total": 200,
        "tax": 0,
        "grandTotal": 200,
        "enterAmountFlag": 1
      }
    ],
    "billingDetails": [
      {
        "billingDetailsId": 54,
        "billingId": 54,
        "tokenId": null,
        "billingType": 1,
        "amount": 100,
        "tax": 0,
        "total": 100,
        "billName": null,
        "customerType": 1,
        "customerId": 2,
        "queueId": 1,
        "billingHead": 0
      },
      {
        "billingDetailsId": 55,
        "billingId": 55,
        "tokenId": null,
        "billingType": 1,
        "amount": 200,
        "tax": 0,
        "total": 200,
        "billName": null,
        "customerType": 1,
        "customerId": 2,
        "queueId": 1,
        "billingHead": 1
      }
    ],
    "paymentModes": [
      {
        "paymentModeName": "Net Banking/Credit/Debit Card",
        "paymentModeIcon": null,
        "paymentModeCode": "NB",
        "paymentModeId": "2",
        "gatewayName": "razorpay"
      },
      {
        "paymentModeName": "Paytm Wallet",
        "paymentModeIcon": null,
        "paymentModeCode": "PAYTM",
        "paymentModeId": "6",
        "gatewayName": "paytm"
      }
    ]
  }
}
```

This endpoint is used when both Business (consultation) and Subscription (user registration) charges are required to be paid together from App

### HTTP Request

`POST http://api.sminq.com/v1/split/payment/create`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
customerType | true | 1 for user.
customerId | true | unique ID for user.
countryId | true | Unique country ID of the business.
tokenId | false | Unique token ID.
charges | true | (Type: Array Object) Payment details

Charges need to have:

Parameter | Default | Description
--------- | ------- | -----------
amount | true | Amount for payment.
billingHead | true | Type of payment ("subscription" or "business").
discount | false | Discount amount (if applied)

### Error codes

Code | Description
--------- | -----------
346 |  Invalid bill amount.
360 |  Invalid billing type.
102 |  Invalid queue ID.
325 |  Invalid customer ID.
325 |  Invalid customer Type.

## Verify Split Online payment

```shell
curl "http://api.sminq.com/v1/split/payment/verify"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "paymentOrderId": null,
    "paymentMode": "PAYTM",
    "paymentGatewayId": "57",
    "paymentStatus": "created",
    "paymentBank": null,
    "paymentGateway": "paytm",
    "paymentQueueId": 1,
    "paymentGatewayOrder": "57",
    "charges": [
      {
        "billingHead": "business",
        "amount": 100,
        "billingId": 57,
        "gatewayOrderId": null
      },
      {
        "billingHead": "subscription",
        "amount": 200,
        "billingId": 58,
        "gatewayOrderId": null
      }
    ]
  }
}
```

This endpoint is used when both Business (consultation) and Subscription (user registration) charges are required to be paid together from App. This is used to verify the amount user is paying online.

### HTTP Request

`POST http://api.sminq.com/v1/split/payment/verify`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue ID.
customerType | true | 1 for user.
customerId | true | unique ID for user.
tokenId | false | Unique token ID.
paymentMode | true | Payment Mode selected for transaction
newVersion | false | To be used by new apps
charges | true | (Type: Array Object) Payment details

Charges need to have:

Parameter | Default | Description
--------- | ------- | -----------
amount | true | Amount for payment.
billingHead | true | Type of payment ("subscription" or "business").
billingId | true | Billing Id as returned by split/payment/create API.

### Error codes

Code | Description
---- | -----------
362  |  Invalid amount.

## Confirm Split Online payment

```shell
curl "http://api.sminq.com/v1/split/payment/confirm"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "invoices": [
      null,
      null
    ],
    "token": {
      "tokenId": 244,
      "tokenQueueId": 1,
      "totalProcessTime": null,
      "tokenUser": null,
      "createdOn": null,
      "joinDate": 1481976988000,
      "joinTime": "15:30:00",
      "appType": null,
      "isAdvance": null,
      "isConfirmed": null,
      "tokenUserGroup": null,
      "uuid": null,
      "tokenNumber": 605
    }
  }
}
```

This endpoint is used when both Business (consultation) and Subscription (user registration) charges are required to be paid together from App. This is used to confirm the payment status as returned from the gateway.

### HTTP Request

`POST http://api.sminq.com/v1/split/payment/confirm`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
paymentOrderId | true | The appointment id for which payment is done.
paymentMode | true | payment mode for appointment.
paymentGatewayId | true | the gateway transaction id.
paymentStatus | true | the payment status returned by gateway.
paymentQueueId | true | the business queue accepting payments.
paymentBank | false | bank for payment
paymentGateway | true | the payment gateway used for payment.
paymentGatewayOrder | true | gateway order id as returned by api
charges | true | (Type: Array Object) Payment details

Charges need to have:

Parameter | Default | Description
--------- | ------- | -----------
amount | true | Amount for payment.
billingHead | true | Type of payment ("subscription" or "business").
billingId | true | Billing Id as returned by split/payment/create API.

Code | Description
--------- | -----------
360 |  Invalid billing id.
361 |  Invalid payment mode.
362 |  Invalid payment amount.

## Send payment link

> Send a payment link to user email and mobile, with a specified amount.

```shell
curl "http://api.sminq.com/v1/business/payment/link"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "invoice": null,
    "token": {
      "tokenId": 8023,
      "tokenQueueId": 13
    }
  }
}
```

This endpoint send a payment link with amount to user mobile and email.

### HTTP Request

`POST http://api.sminq.com/v1/business/payment/link`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue Id.
tokenId | false | Unique token id, in case of pre-payment this id generated later.
customerId | true | customer unique id.
customerType | true | customer type 1 - User 2 - business.
countryId | true | 
amount | true | amount to be sent via payment link.

### Error codes

Code | Description
--------- | -----------
362 | Invalid amount.
102 | Invalid queue Id
110 | Invalid customer ID
359 | Invalid customer type

## Send split payment link

> Send a payment link to user email and mobile, for either business charges or subscription or both.

```shell
curl "http://api.sminq.com/v1/business/payment/link"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "invoices": [
      null,
      null
    ],
    "token": {
      "tokenId": 276,
      "tokenQueueId": 1,
      "totalProcessTime": null,
      "tokenUser": null,
      "createdOn": null,
      "joinDate": null,
      "joinTime": null,
      "appType": null,
      "isAdvance": null,
      "isConfirmed": null,
      "tokenUserGroup": null,
      "uuid": null,
      "tokenNumber": null
    }
  }
}
```

This endpoint send a payment link with amount to user mobile and email. API accepts 2 types of charges, "business" and "subscription". 
Amount sent to user is total of the 2 charge types.

### HTTP Request

`POST http://api.sminq.com/v1/split/payment/link`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Unique business queue Id.
tokenId | false | Unique token id, in case of pre-payment this id generated later.
customerId | true | customer unique id.
customerType | true | customer type 1 - User 2 - business.
countryId | true | 
charges | true | amount to be sent via payment link.

### Error codes

Code | Description
--------- | -----------
362 |  Invalid amount.
102 | Invalid queue Id
110 | Invalid customer ID
359 | Invalid customer type

## Payments summary

> Get the payment summary for a specific token, list of all successful and failed payments.

```shell
curl "http://api.sminq.com/v1/user/payments/summary"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "billingId": 100,
      "billingDate": "2016-10-14",
      "tokenId": 7,
      "billingType": 0,
      "amount": 900,
      "tax": 0,
      "total": 900,
      "billName": null,
      "queueId": 1,
      "queueName": "ENT",
      "invoiceId": 100,
      "invoiceDate": "2016-10-14",
      "isPaid": 1,
      "paymentMode": null,
      "paymentStatus": null,
      "paymentDate": null,
      "paymentGateway": null,
      "invoiceLink": null,
      "paymentGatewayId": "pay_123456789",
      "billingHead": "business",
      "refundAmount": null,
      "discountAdjusted": 20
    }
  ]
}
```

This endpoint returns list of all cash/online payments in completed/pending/failed state.

### HTTP Request

`POST http://api.sminq.com/v1/user/payments/summary`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
tokenId | true | Unique token id.


### Error codes

Code | Description
--------- | -----------
103 | Invalid tokenId

## All User Payments

> Get all the successful payments done by the user via Sminq app. This captures both cash and online payments. 

```shell
curl "http://api.sminq.com/v1/user/payments/all"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "userAccount": {
      "userId": 7504,
      "balance": 2000,
      "expiryDate": "2017-12-26"
    },
    "userTransactions": [
      {
        "billingId": null,
        "invoiceId": null,
        "billingHead": "business",
        "total": 100,
        "refundAmount": null,
        "invoiceDate": "2016-12-28",
        "isPaid": 2,
        "queueName": "Dr. Adams Apple",
        "tokenId": 17491,
        "billingType": 1,
        "joinTime": "10:15:00",
        "joinDate": "2016-12-28"
      },
      {
        "billingId": null,
        "invoiceId": null,
        "billingHead": "subscription",
        "total": 180,
        "refundAmount": null,
        "invoiceDate": "2016-12-28",
        "isPaid": 1,
        "queueName": "Dr. Adams Apple",
        "tokenId": 17491,
        "billingType": 1,
        "joinTime": "10:15:00",
        "joinDate": "2016-12-28"
      }
    ]
  }
}
```

This endpoint returns all successful transactions for a given user.

### HTTP Request

`GET http://api.sminq.com/v1/user/payments/all`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
userId | true | Unique user id.
limit | false | Number of user records to be fetched. Default = 10

### Error codes

Code | Description
--------- | -----------
110 | Invalid User Id

#Pre Payment

## Get Scheduled slots

> Get the list of available slots to schedule token.


```shell
curl "http://api.sminq.com/v1/token/schedule/time"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json

{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "slotId": 60900,
      "queueId": 48,
      "startTimeSlot": "09:00:00",
      "endTimeSlot": "10:00:00",
      "tokenUpperLimit": null,
      "sequence": 1,
      "status": null,
      "groupId": 422,
      "openToUser": 1,
      "premiumSlot": 0,
      "usedSlots": 2,
      "totalSlots": 1,
      "groupStartTime": "09:00:00",
      "groupEndTime": "13:00:00",
      "availableJoinDate": null,
      "groupName": null
    },
    {
      "slotId": 61000,
      "queueId": 48,
      "startTimeSlot": "10:00:00",
      "endTimeSlot": "11:00:00",
      "tokenUpperLimit": null,
      "sequence": 2,
      "status": null,
      "groupId": 422,
      "openToUser": 1,
      "premiumSlot": 0,
      "usedSlots": 1,
      "totalSlots": 1,
      "groupStartTime": "09:00:00",
      "groupEndTime": "13:00:00",
      "availableJoinDate": null,
      "groupName": null
    },
    {
      "slotId": 61100,
      "queueId": 48,
      "startTimeSlot": "11:00:00",
      "endTimeSlot": "12:00:00",
      "tokenUpperLimit": null,
      "sequence": 3,
      "status": null,
      "groupId": 422,
      "openToUser": 1,
      "premiumSlot": 0,
      "usedSlots": 0,
      "totalSlots": 1,
      "groupStartTime": "09:00:00",
      "groupEndTime": "13:00:00",
      "availableJoinDate": null,
      "groupName": null
    },
    {
      "slotId": 61200,
      "queueId": 48,
      "startTimeSlot": "12:00:00",
      "endTimeSlot": "13:00:00",
      "tokenUpperLimit": null,
      "sequence": 4,
      "status": null,
      "groupId": 422,
      "openToUser": 1,
      "premiumSlot": 0,
      "usedSlots": 0,
      "totalSlots": 1,
      "groupStartTime": "09:00:00",
      "groupEndTime": "13:00:00",
      "availableJoinDate": null,
      "groupName": null
    }
  ]
}
```
This endpoint blocks a slot to schedule a token after successful payment.

### HTTP Request

`GET http://api.sminq.com/v1/token/schedule/time`

### Get Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Queue for appointment.
date | true | Date of joining.

## Get Scheduled time groups

> Get the list of available slots to schedule token, grouped by time group.


```shell
curl "http://api.sminq.com/v1/token/schedule/groups"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json

{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "groupName": "",
      "groupId": 2,
      "groupStartTime": "11:00:00",
      "groupEndTime": "15:00:00",
      "slots": [
        {
          "slotId": 21100,
          "queueId": 1,
          "startTimeSlot": "11:00:00",
          "endTimeSlot": "11:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 2,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 50,
          "groupStartTime": "11:00:00",
          "groupEndTime": "15:00:00",
          "availableJoinDate": null,
          "groupName": null
        }
      ]
    },
    {
      "groupName": "",
      "groupId": 9,
      "groupStartTime": "17:00:00",
      "groupEndTime": "21:00:00",
      "slots": [
        {
          "slotId": 21700,
          "queueId": 1,
          "startTimeSlot": "17:00:00",
          "endTimeSlot": "17:30:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 9,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 50,
          "groupStartTime": "17:00:00",
          "groupEndTime": "21:00:00",
          "availableJoinDate": null,
          "groupName": null
        },
        {
          "slotId": 21730,
          "queueId": 1,
          "startTimeSlot": "17:30:00",
          "endTimeSlot": "18:00:00",
          "tokenUpperLimit": null,
          "sequence": 0,
          "status": null,
          "groupId": 9,
          "openToUser": 1,
          "premiumSlot": 0,
          "usedSlots": 0,
          "totalSlots": 50,
          "groupStartTime": "17:00:00",
          "groupEndTime": "21:00:00",
          "availableJoinDate": null,
          "groupName": null
        }
      ]
    }
  ]
}
```
This endpoint blocks a slot to schedule a token after successful payment.

### HTTP Request

`GET http://api.sminq.com/v1/token/schedule/groups`

### Get Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Queue for appointment.
date | true | Date of joining.

## Schedule token

> Schedule a token to be created after successful payment.


```shell
curl "http://api.sminq.com/v1/schedule/appointment"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json

{
  "success": true,
  "httpCode": 200,
  "status": 
    {
      "queueId": 60900,
      "userId": 48,
      "groupId": 33,
      "joinTime": "10:00:00",
      "joinDate": "2016-12-25",
      "appType": 1,
      "slotId": 123456,
      "status":1
    }
}
```
This endpoint blocks a slot byt scheduling a token, another user will view total_slots-1 for joining .

### HTTP Request

`POST http://api.sminq.com/v1/schedule/appointment`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Queue for appointment.
userId | true | User.
groupId | true | User member.
joinTime | true | Join time.
joinDate | true | Join Date.
appType | true | app type of user.
slotId | true | Slot selected.
status | true | status=1.

### Error codes

Code | Description
--------- | -----------
320 | User scheduled a token

## Cancel schedule token

> Cancel a scheduled token.


```shell
curl "http://api.sminq.com/v1/schedule/appointment/cancel"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json

{
  "success": true,
  "httpCode": 200,
  "status": 
    {
      "queueId": 60900,
      "userId": 48,
      "groupId": 33,
      "joinTime": "10:00:00",
      "joinDate": "2016-12-25",
      "appType": 1,
      "slotId": 123456,
      "status":1
    }
}
```
This endpoint releases the slot blocked for a scheduled token, usually happens if payment fails or user goes back.

### HTTP Request

`POST http://api.sminq.com/v1/schedule/appointment/cancel`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Queue for appointment.
userId | true | User.
groupId | true | User member.
joinTime | true | Join time.
joinDate | true | Join Date.
appType | true | app type of user.
slotId | true | Slot selected.
status | true | status=0.


#Miscellaneous

## User contact

> Capture user contact form data.

```shell
curl "http://api.sminq.com/v1/web/contact"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "name": "sheldon",
    "email": "email",
    "mobile": "9890005358",
    "message": "demo",
    "googleForm": null
  }
}
```

This endpoint captures user contact us form input.

### HTTP Request

`POST http://api.sminq.com/v1/web`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
name | true | User name.
mobile | true | user mobile.
email | true | user email.
message | true | user message

### Error codes

Code | Description
--------- | -----------
336 | User name cannot be empty
337 | User mobile cannot be empty
335 | Message cannot be empty

## City config

> Get city configuration.

```shell
curl "http://api.sminq.com/v1/city/config"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "cityName": "Pune",
      "cityId": "1",
      "categories": [
        {
          "categoryName": "Clinic",
          "categoryIconUrl": null,
          "categoryStatus": 1,
          "createdBy": null,
          "categoryCityId": null,
          "verticalType": "clinic",
          "sortOrder": null
        },
        {
          "categoryName": "Restaurant",
          "categoryIconUrl": null,
          "categoryStatus": 0,
          "createdBy": null,
          "categoryCityId": null,
          "verticalType": "restaurant",
          "sortOrder": null
        },
        {
          "categoryName": "Salon",
          "categoryIconUrl": null,
          "categoryStatus": 0,
          "createdBy": null,
          "categoryCityId": null,
          "verticalType": "salon",
          "sortOrder": null
        },
        {
          "categoryName": "Walkin Interview",
          "categoryIconUrl": null,
          "categoryStatus": 0,
          "createdBy": null,
          "categoryCityId": null,
          "verticalType": "walkin",
          "sortOrder": null
        }
      ],
      "tags": [
        {
          "tagId": 1,
          "tagName": "Dermatologist"
        },
        {
          "tagId": 2,
          "tagName": "General Practice"
        },
        {
          "tagId": 5,
          "tagName": "Pediatrician"
        },
        {
          "tagId": 4,
          "tagName": "Orthopedic"
        },
        {
          "tagId": 3,
          "tagName": "Gynecologist"
        },
        {
          "tagId": 6,
          "tagName": "Demo"
        },
        {
          "tagId": 9,
          "tagName": "Ayurvedic"
        },
        {
          "tagId": 10,
          "tagName": "General Medicine"
        },
        {
          "tagId": 11,
          "tagName": "ENT"
        },
        {
          "tagId": 12,
          "tagName": "Physiotherapists"
        },
        {
          "tagId": 13,
          "tagName": "General Surgeon"
        },
        {
          "tagId": 14,
          "tagName": "Ophthalmologist"
        },
        {
          "tagId": 15,
          "tagName": "Neurologist"
        },
        {
          "tagId": 16,
          "tagName": "Diabetologist"
        },
        {
          "tagId": 17,
          "tagName": "Cardiologists"
        },
        {
          "tagId": 18,
          "tagName": "Dentist"
        },
        {
          "tagId": 8,
          "tagName": "Test"
        },
        {
          "tagId": 19,
          "tagName": "Homeopath"
        }
      ],
      "languages": [
        {
          "cityId": 1,
          "languageId": 1,
          "isDefault": 1,
          "languageName": "English"
        },
        {
          "cityId": 1,
          "languageId": 2,
          "isDefault": 0,
          "languageName": "मराठी"
        }
      ],
      "locations": [
        {
          "locationId": 25,
          "locationName": "Akurdi",
          "locationLat": 18.760099411010742,
          "locationLong": 73.69190216064453,
          "cityId": null
        },
        {
          "locationId": 7,
          "locationName": "Aundh",
          "locationLat": 18.557899475097656,
          "locationLong": 73.80850219726562,
          "cityId": null
        },
        {
          "locationId": 23,
          "locationName": "Bhandarkar Road",
          "locationLat": 18.518699645996094,
          "locationLong": 73.83429718017578,
          "cityId": null
        }
      ]
    },
    {
      "cityName": "TestCity",
      "cityId": "3",
      "categories": [
        {
          "categoryName": "Bank",
          "categoryIconUrl": null,
          "categoryStatus": 1,
          "createdBy": null,
          "categoryCityId": null,
          "verticalType": "bank",
          "sortOrder": null
        }
      ],
      "tags": [
        {
          "tagId": 20,
          "tagName": "HDFC"
        }
      ],
      "languages": [],
      "locations": [
        {
          "locationId": 35,
          "locationName": "Thane",
          "locationLat": null,
          "locationLong": null,
          "cityId": null
        }
      ]
    }, 
    {
      "cityName": "Mumbai",
      "cityId": "8",
      "categories": [
        {
          "categoryName": "Bank",
          "categoryIconUrl": null,
          "categoryStatus": 1,
          "createdBy": null,
          "categoryCityId": null,
          "verticalType": "bank",
          "sortOrder": null
        }
      ],
      "tags": [
        {
          "tagId": 20,
          "tagName": "HDFC"
        }
      ],
      "languages": [],
      "locations": [
        {
          "locationId": 40,
          "locationName": "MALAD WEST - ORLEM",
          "locationLat": null,
          "locationLong": null,
          "cityId": null
        }
      ]
    }
  ]
}
```

This endpoint captures user contact us form input.

### HTTP Request

`GET http://api.sminq.com/v1/city/config`

### Get Parameters

Parameter | Default | Description
--------- | ------- | -----------
cityName | false | required only if GPS is activated, to get city name.


# Monetization
## Create Token For Monetization Queue

> Create Token :


```shell
curl "https://api.sminq.com/v1/user/monetization/"
  -H "Authorization: XXXXXXXXXX"
```

> The above api returns JSON structure likes this if monetization plan does not apply to this user

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "monetizationPlanDetails": {
      "monetizationId": 1,
      "monetizationDescription": "Prime on Weekend morning and evening",
      "monetizationPlanName": "",
      "chargeType": 0,
      "registrationPlans": null,
      "bookingCharge": 0,
      "isUserMembershipValid": false
    },
    "token": {
      "tokenId": 243,
      "tokenQueueId": 1,
      "totalProcessTime": null,
      "tokenUser": 7,
      "createdOn": null,
      "joinDate": 1481976454000,
      "joinTime": "15:30:00",
      "appType": null,
      "isAdvance": null,
      "isConfirmed": null,
      "tokenUserGroup": null,
      "uuid": null,
      "tokenNumber": 604
    },
    "userValid": true
  }
}
```

> The above api returns JSON structure likes this if Registration plans are applicable but User has Credits (userValid = true):

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "monetizationPlanDetails": {
      "monetizationId": 1,
      "monetizationDescription": "Prime on Weekend morning and evening",
      "monetizationPlanName": "",
      "chargeType": 0,
      "registrationPlans": null,
      "bookingCharge": 0,
      "isUserMembershipValid": true,
    },
    "token": {
      "tokenId": 242,
      "tokenQueueId": 1,
      "totalProcessTime": null,
      "tokenUser": 5,
      "createdOn": null,
      "joinDate": 1481975813000,
      "joinTime": "15:30:00",
      "appType": null,
      "isAdvance": null,
      "isConfirmed": null,
      "tokenUserGroup": null,
      "uuid": null,
      "tokenNumber": 603
    }
  }
}
```

> The above api returns JSON structured like this if Flat Fee (booking charge) is applicable:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "monetizationPlanDetails": {
      "monetizationId": 1,
      "monetizationDescription": "Prime on Weekend morning and evening",
      "monetizationPlanName": "",
      "chargeType": 1,
      "registrationPlans": null,
      "bookingCharge": 500,
      "isUserMembershipValid": false
    },
    "token": null,
    "userValid": false
  }
}

```

This endpoint needs to be used only for the queues which has monetization plan applicable (isMonetizationApplicable = 1 in /queue/profile response). Here, monetization plan and it's configuration is validated for the input parameters. If monetization plan is not applicable for the given user then a token is immediately created. But if validation results in applicability of monetization on the requesting user then a charge that user has to pay needs to be computed. Charge structure can be either Registration Plan or Flat Fee. 

Flat Fee: Here, user has to pay first and then only a token gets created for that user. In response of this API call, server returns the amount to be paid.

Registration Plans: In this case, token gets created for that user but if user is not a member or existing membership is not valid then along with token details, registration plans are also returned back to the client.

### HTTP Request

`POST http://api.sminq.com/v1/user/monetization/`

### POST Parameters

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Business queueId for appointment.
userId | true | If user is logged in send across id.
joinTime | true | Time for appointment.
joinDate | true | Date for appointment.
appType | true | appointment created from which channel 
categoryId | true | vertical of the selected business
cityId | true | City of the selected business
countryId | true | Country of the selected business
slotId | false | Slot for appointment.

### Error codes

Code | Description
---- | -----------
102  |  Invalid queue ID.
326  |  Business blocked appointments.
322  |  Invalid join date.
323  |  Invalid join time.
324  |  Advance bookings not accepted.
325  |  Invalid user.

## Get Monetization Charges

> Get applicable monetization charges for a given queue (if applicable) and user. Monetization charges will vary as per chargeType. If for a given queue chargeType is registration then this API will return all the plans. But if chargeType is flat-fee then it will compute booking charge for the given user and return applicable amount.

```shell
curl "http://api.sminq.com/v1/user/monetization/charges"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this, if Registration Charges are applicable for the given queue id:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "monetizationId": null,
    "monetizationDescription": null,
    "monetizationPlanName": null,
    "chargeType": 0,
    "registrationPlans": [
      {
        "planName": "Gold",
        "planDescription": "Unlimited Token Booking|2000 Credits|Priority Support",
        "amount": 200,
        "validity": 365,
        "recommended": 1,
        "consultationDiscount": 200,
        "consultationDiscountValidity": 30
      },
      {
        "planName": "Silver",
        "planDescription": "Unlimited Token Booking|500 Credits|Priority Support",
        "amount": 50,
        "validity": 30,
        "recommended": 0,
        "consultationDiscount": 50,
        "consultationDiscountValidity": 30
      }
    ],
    "bookingCharge": 0,
    "onlineDiscount": 10,
    "isUserMembershipValid": true,
    "userAccountSummary": {
      "userId": 21,
      "balance": 500,
      "expiryDate": "2017-02-08"
    }
  }
}

```
> If booking charge is applicable then JSON structure would be like:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "monetizationId": null,
    "monetizationDescription": null,
    "monetizationPlanName": null,
    "chargeType": 1,
    "registrationPlans": null,
    "bookingCharge": 20,
    "onlineDiscount": 0
  }
}
```

This endpoint first determines chargeType applicable for the given queue id and accordingly returns either booking charge for the given user or registration plans for the given queue.

### HTTP Request

`GET http://api.sminq.com/v1/user/monetization/charges`

Parameter | Default | Description
--------- | ------- | -----------
queueId | true | Queue ID for which plans are to be fetched.
userId | true | User ID for which booking charge and membership status is to be fetched

### Error codes

Code | Description
--------- | -----------
102 | Invalid queue Id
110 | Invalid user ID

## Get User Account Summary

> Get User Account Summary

```shell
curl "http://api.sminq.com/v1/user/account/summary"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": {
    "userId": 5,
    "balance": 1000,
    "expiryDate": "2017-01-01"
  }
}
```

> If user does not have a membership then JSON structure returned is:

```json
{
  "success": true,
  "httpCode": 200,
  "status": null
}
```

This endpoint returns credit balance of the user along with expiry date in yyyy-mm-dd format.

### HTTP Request

`GET http://api.sminq.com/v1/user/account/summary`

Parameter | Default | Description
--------- | ------- | -----------
userId | true | User ID whose account summary is to be fetched

### Error codes

Code | Description
--------- | -----------
110 | Invalid user ID

## Get All User Monetization Transactions

> Get list of all monetization related transactions (points credit/debit) for a user.

```shell
curl "http://api.sminq.com/v1/user/account/details"
  -H "Authorization: xxxxxx"
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "httpCode": 200,
  "status": [
    {
      "monetizationId": 1,
      "planName": "Test Plan - All Users",
      "queueName": "Dr. Adams Apple",
      "userId": 7247,
      "queueId": 101,
      "isPaid": 1,
      "credits": 2000,
      "tokenId": 17227,
      "userAction": "T_CREATE",
      "joinDate": 1481559412000,
      "joinTime": "19:35:00"
    },
    {
      "monetizationId": 1,
      "planName": "Test Plan - All Users",
      "queueName": "Dr. Adams Apple",
      "userId": 7247,
      "queueId": 101,
      "isPaid": 1,
      "credits": 200,
      "tokenId": 17227,
      "userAction": "T_CANCEL",
      "joinDate": 1481647518000,
      "joinTime": "23:35:00"
    },
    {
      "monetizationId": 1,
      "planName": "Test Plan - All Users",
      "queueName": "Dr. Adams Apple",
      "userId": 7247,
      "queueId": 101,
      "isPaid": 1,
      "credits": 100,
      "tokenId": 17227,
      "userAction": "T_RESCHEDULE",
      "joinDate": 1481647518000,
      "joinTime": "23:35:00"
    }
  ]
}
```

This endpoint returns history of credit or debit of user points under monetization.

### HTTP Request

`GET http://api.sminq.com/v1/user/account/details`

Parameter | Default | Description
--------- | ------- | -----------
userId | true | User ID whose account details are to be fetched
limit | false | number of records to be fetch. Default = 10

### Error codes

Code | Description
--------- | -----------
110 | Invalid user ID