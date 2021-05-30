---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='https://www.shipnerd.com/account'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

**Welcome to ShipNerd's API**

ShipNerd makes shipping quick, easy and affordable. No hidden fees, no volume commitments, huge discounts and we only ship with the best in the business. Get your free account today, and make the smarter shipping choice.

Our shipping API enables you to instantly obtain shipping quotes and create labels. It's quick & easy, follow the instructions and start shipping today.

# Authentication

```javascript
const request = require('request')

request.post({
    url: [API_URL],
    headers: {
      'Authorization': 'JWT [API_TOKEN]'
    },
    json: true,
    body: ...
}, function(err, response, body){}) 
```
> Make sure to replace `[API_TOKEN]` with your API token.

The API supports JWT tokens for authentication.

After you signup for a free acount, go to the "Account Settings" page and enable the API to get your token.

![Shipnerd api section](images/api-section.png)

ShipNerd expects the API token to be included in all API requests as the Authorization header:

`Authorization: JWT [API_TOKEN]`

<aside class="notice">
You must replace <code>[API_TOKEN]</code> with your personal API token.
</aside>

# Interacting with the API

## Get Rates

> Request:

```javascript
const request = require('request');

var shipments = [];
var shipment = {
  from:{
    name: 'John Doe',
    company: 'Acme Labs',
    country: 'US',
    address1: '111 8th Ave',
    address2: '',
    zipCode: '10011',
    city: 'New York',
    state: 'NY',
    phone: '1234567890',
    email: 'john@acmelabs.com'  
  },
  to:{
    name: 'Ned Flanders',
    company: 'Simpsons Labs',
    country: 'US',
    address1: '300 Post St',
    address2: 'Suite 152',
    zipCode: '94108',
    city: 'San Francisco',
    state: 'CA',
    phone: '1234567890',
    email: 'ned@simpsons.com' 
  },
  referenceNumber: 'Nerds Rule',
  packagingType: 'your_packaging',
  weightUnits: 'lbs',
  dimensionUnits: 'in',
  packages: [{
    weight: 10
  },
  {
    weight: 12,
    width: 12,
    length: 12,
    height: 12
  }]
}
shipments.push(shipment);

request.post({
    url: "https://api.shipnerd.com/rates",
    headers: {
      'Authorization': 'JWT [API_TOKEN]'
    },
    body: shipments,
    json: true
}, function(err, response, body){
    if (err){
        .....
    }
    else{
      .....
    }
}) 
```

> Response:

```json
{
  "status": true,
  "results": [
    {
      "status": true,
      "referenceNumber": "Nerds Rule",
      "rates": [
        {
          "carrier": "[carrier]",
          "serviceCode": "[code]",
          "serviceName": "[service name]",
          "deliveryTime": "Tue December 05",
          "breakdown": {
            "transportation": "80.16",
            "accessorials": [],
            "subtotal": 80.16,
            "taxes": {},
            "total": 80.16
          },
          "currency": "USD",
          "billingWeight": 11,
          "saturdayDelivery": false
        },
        {
          "carrier": "[carrier]",
          "serviceCode": "[code]",
          "serviceName": "[service name]",
          "deliveryTime": "Tue December 05",
          "breakdown": {
            "transportation": "69.87",
            "accessorials": [],
            "subtotal": 69.87,
            "taxes": {},
            "total": 69.87
          },
          "currency": "USD",
          "billingWeight": 11,
          "saturdayDelivery": false
        },
        {
          "carrier": "[carrier]",
          "serviceCode": "[code]",
          "serviceName": "[service name]",
          "deliveryTime": "Wed December 06",
          "breakdown": {
            "transportation": "44.15",
            "accessorials": [],
            "subtotal": 44.15,
            "taxes": {},
            "total": 44.15
          },
          "currency": "USD",
          "billingWeight": 11,
          "saturdayDelivery": false
        },
        {
          "carrier": "[carrier]",
          "serviceCode": "[code]",
          "serviceName": "[service name]",
          "deliveryTime": "Mon December 11",
          "breakdown": {
            "transportation": "13.99",
            "accessorials": [],
            "subtotal": 13.99,
            "taxes": {},
            "total": 13.99
          },
          "currency": "USD",
          "billingWeight": 11,
          "saturdayDelivery": false
        }
      ]
    }
  ]
}


```
Returns rates for the requested shipments

### HTTP Request

`POST https://api.shipnerd.com/rates`

### Body Parameters

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
shipments | N | Array | List of shipments. See [Shipment] (#shipment)

### Response Parameters

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
status | N | Boolean | Status of operation
results | N | Array | Rate results. See [Rate Result] (#rate-result)

<aside class="notice">
Max number of shipments allowed per request is 10 
</aside>

## Create Labels

> Request:

```javascript
const request = require('request');

const shipments = [];

shipments.push({
    from:{
        name: 'John Doe',
        company: 'Acme Labs',
        country: 'US',
        address1: '111 8th Ave',
        address2: '',
        zipCode: '10011',
        city: 'New York',
        state: 'NY',
        phone: '1234567890',
        email: 'john@acmelabs.com'  
    },
    to:{
        name: 'Ned Flanders',
        company: 'Simpsons Labs',
        country: 'US',
        address1: '300 Post St',
        address2: 'Suite 152',
        zipCode: '94108',
        city: 'San Francisco',
        state: 'CA',
        phone: '1234567890',
        email: 'ned@simpsons.com' 
    },
    referenceNumber: 'Nerds Rule',
    packagingType: 'your_packaging',
    weightUnits: 'lbs',
    dimensionUnits: 'in',
    packages: [{
        weight: 10,
        width: 12,
        length: 12,
        height: 12
    }],
    carrierName: '[carrier]',
    serviceCode: '[code]'
})

shipments.push({
    orderId: '1512154689639',
    carrierName: '[carrier]',
    serviceCode: '[code]'
})

shipments.push({
    from: {
       name: 'John Doe',
       company: 'Acme Labs',
       country: 'CA',
       address1: '80 Cumberland',
       address2: '',
       zipCode: 'M5R3V1',
       city: 'Toronto',
       state: 'ON',
       phone: '1234567890',
       email: 'john@acmelabs.com'
    },
    to: {
       name: 'Ned Flanders',
       company: 'Simpsons Labs',
       country: 'US',
       address1: '171 w 71st',
       address2: '',
       zipCode: '10023',
       city: 'New York',
       state: 'NY',
       phone: '1234567890'
    },
    referenceNumber: 'Nerds Rule 1',
    packagingType: 'your_packaging',
    weightUnits: 'lbs',
    dimensionUnits: 'in',
    packages: [{
       weight: 10,
       width: 12,
       length: 12,
       height: 12
    }],
    customsInfo: {
        reasonForExport: 'commercial',
        commodities: [{
            description: 'A white shirt',
            manufactureCountry: 'CA',
            quantity: 1,
            quantityUnits: 'PCS',
            weight: 2,
            weightUnits: 'lbs',
            value: 100
        }]
    },
    carrierName: '[carrier]',
    serviceCode: '[code]'
})

request.post({
    url: "https://api.shipnerd.com/labels",
    headers: {
      'Authorization': 'JWT [API_TOKEN]'
    },
    body: shipments,
    json: true
}, function(err, response, body){
    if (err){
        .....
    }
    else{
      .....
    }
}) 
```

> Response

```json
{
  "status": true,
  "results": [
    {
        "status": true,
        "orderId": "1512154689639",
        "referenceNumber": "Nerds Rule",
        "carrier": "[carrier]",
        "service": "[service]",
        "trackingNumber": "[tracking number]",
        "label": "",
        "packingSlip": "",
        "labelExpirationDate": "Monday, December 11th, 2017 at 11:59 p.m. PST"
    },
    {
        "status": true,
        "orderId": "1512155343427",
        "carrier": "[carrier]",
        "service": "[service]",
        "trackingNumber": "[tracking number]",
        "label": "",
        "packingSlip": "",
        "labelExpirationDate": "Monday, December 11th, 2017 at 11:59 p.m. PST"
    },
    {
        "status": false,
        "referenceNumber": "Nerds Rule 1",
        "message": "International shipments should have customs information"
    }   
  ]
}

```

Creates labels for the requested shipments

### HTTP Request

`POST https://api.shipnerd.com/labels`

### Body Parameters

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
shipments | N | Array | List of shipments. See [Shipment] (#shipment)

### Response Parameters

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
status | N | Boolean | Status of operation
results | N | Array | Label Results. See [Label Result] (#label-results)

<aside class="notice">
Max number of shipments allowed per request is 10 
</aside>

# Entities

## Shipment

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
orderId | Y | String | Will be ignored in rates call. If used in create_labels call - the order will be referenced and all other fields will be ignored.
from | N | Object | Sender's information. See [Address] (#address)
to | N | Object | Receiver's information. See [Address] (#address)
referenceNumber | Y | String | Must be up to 30 chars
packagingType | N | String | Available values are 'your_packaging' and 'envelope'
weightUnits | N | String | Available values are 'lbs' and 'kgs' (Please use lowercase)
dimensionUnits| N | String |  Available values are 'in' and 'cm'; 'lbs' must be used with 'in' and 'kgs' with 'cm'
packages | Cond | Array | List of packages. Should be present only if 'your_packaging' is used as packagingType. See [Package] (#package)
isSignatureRequired | Y | Boolean | Is signature required
isAdultSignatureRequired | Y | Boolean | Is adult signature required
isResidentialAddress | Y | Boolean | Is residential address
carrierName | N | String | Desired carrier name for creating the label. Must be a valid carrier name for the specific shipment. Will be ignored in rates call.
serviceCode | N | String | Desired service code for creating the label. Must be a valid service code for the specific shipment. Will be ignored in rates call.
customsInfo | Cond | Object | Should be present for international shipments. See [Customs] (#customs)

## Address

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
name | N | String | Must be up to 35 chars
company | Y | String | Must be up to 35 chars
address1 | N | String | Must be up to 35 chars
address2 | Y | String | Must be up to 35 chars
city | N | String | Must be up to 30 chars
state | Cond | String | 2 letters state code. Mandatory only for countries with states.
zipCode | N | String | A valid zip code for the address
country | N | String | 2 letters country code. Must be a supported origin and destination. Please check 'Ship Page' for more information.
phone | N | String | Must be between 10-15 chars including 10 digits
email | Cond | String | A valid email address (optional only under "to" field)

## Package

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
weight | N | Number | For 'lbs' min value is 2 and max value is 150. For 'kgs' min value is 1 and max value is 68
length | Y | Whole Number | Must be used together with 'width' & 'height' fields. For 'in' max value is 108. For 'cm' max value is 270. 
width | Y | Whole Number | Must be used together with 'length' & 'height' fields
height | Y | Whole Number | Must be used together with 'width' & 'length' fields
value | Y | Number | Insurance value for package

## Customs

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
isFreeDomicile | Y | Boolean | Sets the shipment as free domicile. Works only if free domicile is enabled for user's account.
chargeAccount | Y | Object | Sets the account to charge duties and taxes on free domicile shipments. See [ChargeAccount] (#charge-account) 
documents | Y | Object | Describes a documents shipment. See [Documents] (#documents)
reasonForExport | Cond | String | Must be used with commodities. Available values are 'commercial', 'gift', 'sample', 'return', 'repair', 'personal_effects' and 'personal_use' (Please use lowercase)
commodities | Y | Array | Describes a non-documents shipment. List of commodities. See [Commodity] (#commodity)

## Documents

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
type | N | String | Available values are 'Documents With No Commercial Value', 'Letters and Cards', 'Interoffice Memos' and 'Business Correspondence' (Please use same case)
value | N | Number | Value of documents. If 'Documents With No Commercial Value' was set as type field - value must be 0

## Commodity

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
description | N | String | Must be up to 35 chars and include at least 2 words
hsCode | Y | String | Must be between 6 to 14 chars
manufactureCountry | N | String | 2 letters country code
quantity | N | Whole Number | Number of pieces
quantityUnits | N | String | Available values are - 'EA' = each, 'PCS' = pieces and 'PRS' = pairs (Please use same case)
weight | N | Number | Commodity weight
weightUnits| N | String | Available values are 'lbs' and 'kgs'. Must be the same unit as the packages' weight 
value | N | Number | Value of commodity

## Charge Account

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
accountNumber | N | String | Account number
postalCode | N | String | Registered account postal code
countryCode | N | String | Registered account country code

## Rate Result

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
status | N | Boolean | Status of operation
referenceNumber | Y | String | Must be up to 30 chars
rates | N | Array | List of rates. See [Rate] (#rate) 

## Rate

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
carrier | N | String | Carrier name
serviceCode | N | String | Serivce code
serviceName | N | String | Service name
deliveryTime | N | String | The estimated delivery time
breakdown | N | Object | Rate breakdown. See [Breakdown] (#breakdown)
currency | N | String | Rate currency
billingWeight | N | Number | The Billing weight rate is based on
saturdayDelivery | N | Boolean | Indicates if the rate is for a saturday delivery

## Breakdown

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
transportation | N | Number | Transportation price
accessorials | N | Array | List of accessorials. See [Accessorial] (#accessorial)
taxes | N | Object | Taxes breakdown. See [Tax] (#tax)
subtotal | N | Number | Subtotal price
total | N | Number | Total price

## Accessorial

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
name | N | String | Accessorial name
price | N | Number | Accessorial price

## Tax

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
name | N | String | Tax name
price | N | Number | Tax price
percentage | N | Number | Tax percentage

## Label Results

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
status | N | Boolean | Status of operation
referenceNumber | Y | String | Must be up to 30 chars
orderId | N | String | The created order ID
carrier | N | String | Carrier name
service | N | String | Service name
trackingNumber | N | String | Shipment tracking number
label | N | String | Url to the generated label
packingSlip | Y | String | Url to the generated packing slip
labelExpirationDate | N | String | Label expiration date

# Limits

## Get Rates

Max of 15 requests per 1 minute per client. 

## Create Labels

Max of 5 requests per 1 minute per client.

<aside class="notice">
Additional requests will result in an HTTP 429 (Too Many Requests) error.
</aside>