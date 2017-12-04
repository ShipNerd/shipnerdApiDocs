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

ShipNerd makes shipping quick, easy, and affordable. No hidden fees, no volume commitments, huge discounts and we only ship with the best in the business. Get your free account today, and make the smarter shipping choice.

Using our API you can get shipping quotes and create labels. It's quick & easy, follow the instructions and start shipping today.

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

After you signup for a free acount, go to "Account Settings" page and enable the API to get your token.

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
  packages: [{
    weight: 10,
    weightUnits: 'lbs'
  },
  {
    weight: 12,
    weightUnits: 'lbs',
    width: 12,
    length: 12,
    height: 12,
    dimensionUnits: 'in'
  }]
}
shipments.push(shipment);

request.post({
    url: "https://shipnerd.com/api/1.0/get_rates",
    headers: {
      'Authorization': 'JWT [API_TOKEN]'
    },
    body: {
      shipments: shipments
    },
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
  "data": [
    {
      "rates": [
        {
          "shipnerdService": "hyperspeed",
          "carrierService": "UPS Next Day Air",
          "deliveryTime": "Tue December 05 by 10:30 am",
          "breakdown": {
            "transportation": "80.16",
            "accessorials": [
              {
                "name": "DELIVERY AREA",
                "price": 0
              }
            ],
            "subtotal": 80.16,
            "taxes": {},
            "total": 80.16
          },
          "currency": "USD",
          "billingWeight": 11
        },
        {
          "shipnerdService": "fastest",
          "carrierService": "UPS Next Day Air Saver",
          "deliveryTime": "Tue December 05 by 03:00 pm",
          "breakdown": {
            "transportation": "69.87",
            "accessorials": [
              {
                "name": "DELIVERY AREA",
                "price": 0
              }
            ],
            "subtotal": 69.87,
            "taxes": {},
            "total": 69.87
          },
          "currency": "USD",
          "billingWeight": 11
        },
        {
          "shipnerdService": "faster",
          "carrierService": "UPS 2nd Day Air",
          "deliveryTime": "Wed December 06 by 11:00 pm",
          "breakdown": {
            "transportation": "44.15",
            "accessorials": [
              {
                "name": "DELIVERY AREA",
                "price": 0
              }
            ],
            "subtotal": 44.15,
            "taxes": {},
            "total": 44.15
          },
          "currency": "USD",
          "billingWeight": 11
        },
        {
          "shipnerdService": "fast",
          "carrierService": "UPS Ground",
          "deliveryTime": "Mon December 11 by 11:00 pm",
          "breakdown": {
            "transportation": "13.99",
            "accessorials": [
              {
                "name": "DELIVERY AREA",
                "price": 0
              }
            ],
            "subtotal": 13.99,
            "taxes": {},
            "total": 13.99
          },
          "currency": "USD",
          "billingWeight": 11
        }
      ]
    }
  ]
}


```
Returns rates for the requested shipments

### HTTP Request

`POST https://www.shipnerd.com/api/1.0/get_rates`

### Body Parameters

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
shipments | N | Array | List of shipments. See [Shipment] (#shipment)

### Response Parameters

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
status | N | Boolean | Status of operation
data | N | Array | Rate results. See [Rate Results] (#rate-results)

<aside class="notice">
Max number of shipments allowed per request is 10 
</aside>

## Create Labels

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
  packages: [{
    weight: 10,
    weightUnits: 'lbs',
    width: 12,
    length: 12,
    height: 12,
    dimensionUnits: 'in'
  }],
  shipnerdService: 'hyperspeed'
}
shipments.push(shipment);

shipments.push({
  orderId: '1512154689639',
  shipnerdService: 'faster'
})

request.post({
    url: "https://shipnerd.com/api/1.0/create_labels",
    headers: {
      'Authorization': 'JWT [API_TOKEN]'
    },
    body: {
      shipments: shipments
    },
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
  "data": [
    {
      "orderId": "1512154689639",
      "label": "https://www.shipnerd.com/api/website/label/get_pdf?orderId=1512154689639&mediaId=Byx8kFQ1Zz57cf395fc8effbb528e279b9"
    },
    {
      "orderId": "1512155343427",
      "label": "https://www.shipnerd.com/api/website/label/get_pdf?orderId=1512155343427&mediaId=r1G81Y7y-G57cf395fc8effbb528e279b9"
    }
  ]
}

```

Creates labels for the requested shipments

### HTTP Request

`POST https://www.shipnerd.com/api/1.0/create_labels`

### Body Parameters

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
shipments | N | Array | List of shipments. See [Shipment] (#shipment)

### Response Parameters

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
status | N | Boolean | Status of operation
data | N | Array | Label Results. See [Label Results] (#label-result)

<aside class="notice">
Max number of shipments allowed per request is 10 
</aside>

# Entities

## Shipment

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
orderId | Y | String | Will be ignored in rates call. If used in create_labels call - the order will referenced and all other fields will be ignored. Unless used with 'isUpdate' field
isUpdate | Y | Boolean | Only used together with OrderId. If set to true, all other fields must be specified (from, to, packagingType ... ) so that the order will be updated with new values
from | N | Object | Sender's information. See [Address] (#address)
to | N | Object | Receiver's information. See [Address] (#address)
packagingType | N | String | 'your_packaging' or 'envelope'
packages | Cond | Array | Array of packages. Should be present only if 'your_packaging' is used as packagingType. See [Package] (#package)
isSignature | Y | Boolean | Is signature required
isResidential | Y | Boolean | Is residential address
referenceNumber | Y | String | Must be up to 30 chars
service | N | String | Available values are 'hyperspeed', 'fastest', 'faster' and 'fast'. Not all services are availale for every shipment. Will be ignored in rates call.
customsInfo | Cond | Object | Should be present for international shipments. See [Customs] (#customs)

## Address

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
name | N | String | Must be up to 35 chars
company | N | String | Must be up to 35 chars
address1 | N | String | Must be up to 35 chars
address2 | N | String | Must be up to 35 chars
city | N | String | Must be up to 30 chars
state | Y | String | 2 letters state code. Mandatory only for countries with state.
zipCode | N | String | A valid zip code for the address
country | N | String | 2 letters country code. We currently only support CA->CA, CA->US, CA->IL, CA->GB, CA->FR, CA->AU, CA->CN, CA->DE, CA->GR, CA->IT, CA->JP, CA->ES, CA->CH, CA->PH, CA->PK, CA->SE, CA->IN, CA->MX, CA->BR, US->US, US->CA
phone | N | String | Must be between 10-15 chars including 10 digits
email | N | String | A valid email address

## Package

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
weight | N | Number | For 'lbs' min value is 2 and max value is 150. For 'kgs' min value is 1 and max value is 68
weightUnits | N | String | Must be 'lbs' or 'kgs' in lowercase
length | Y | Whole Number | Can only be used together with 'width' & 'height'. For 'in' must be under than 108. For 'cm' must be under 270. 
width | Y | Whole Number | Can only be used together with 'length' & 'height'
height | Y | Whole Number | Can only be used together with 'width' & 'length'
dimensionUnits| Cond | String | Must be used when dimensions are specified. 'lbs' goes with 'in' and 'kgs' goes with 'cm'
value | Y | Number | Declared value of package

## Customs

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
documents | Y | Object | Describes a documents shipment. See [Documents] (#documents)
commodities | Y | Object | Describes a non-documents shipment. See [Commodities] (#commodities)

## Documents

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
type | N | String | Available values are 'Documents With No Commercial Value', 'Letters and Cards', 'Interoffice Memos' or 'Business Correspondence'
value | N | Number | Value of documents. If 'Documents With No Commercial Value' was set, value must be 0

## Commodities

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
reasonForExport | N | String | Available values are 'commercial', 'gift', 'sample', 'return', 'repair', 'personal_effects' or 'personal_use'
commodities | N | Array | List of commodities. See [Commodity] (#commodity)

## Commodity

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
description | N | String | Must be up to 35 chars and include at least 2 words
manufactureCountry | N | String | 2 letters country code of manufacture
quantity | N | Whole Number | Number of pieces
quantityUnits | N | Whole Number | Available values are - 'EA' = each, 'PCS' = pieces & 'PRS' = pairs
weight | N | Number | Commodity weight
weightUnits| N | String | Must be 'lbs' or 'kgs' in lower case 
value | N | Number | Value of commodity

## Rate Results

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
rates | N | Object | List of rates. See [Rate] (#rate) 

## Rate

Parameter | Optional | Type | Description
--------- | -------- | ---- | -----------
shipnerdService | N | String | ShipNerd service. Available values are hyperspeed, fastest, faster and fast
carrierService | N | String | The carrier (UPS, Fedex or DHL) service
breakdown | N | Object | The rate breakdown. See [Breakdown] (#breakdown)
currency | N | String | The rate currency
billingWeight | N | Number | Billing weight the rate is based on

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
orderId | N | String | The created order ID
label | N | String | Url to the generated label

# Limits

Maximum of 10 requests per 5 minutes per client. 
Additional requests will result in an HTTP 429 (Too Many Requests) error.
