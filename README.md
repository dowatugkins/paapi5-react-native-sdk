# Product Advertising API 5.0 SDK for react-native (v1)

[![NPM](https://nodei.co/npm/paapi5-react-native-sdk.svg?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/paapi5-react-native-sdk/)

[![Version](https://badge.fury.io/js/paapi5-react-native-sdk.svg)](http://badge.fury.io/js/paapi5-react-native-sdk) [![npm](https://img.shields.io/npm/dt/paapi5-react-native-sdk.svg)](https://www.npmjs.com/package/paapi5-react-native-sdk)

This repository contains the official Product Advertising API 5.0 react-native SDK called **paapi5-react-native-sdk** that allows you to access the [Product Advertising API](https://webservices.amazon.com/paapi5/documentation/index.html) from your react-native app.

It is forked from the Node.js repo at [Product Advertising API](https://webservices.amazon.com/paapi5/documentation/index.html). paapi5-nodejs-sdk can be found on [npm](https://www.npmjs.com/package/paapi5-react-native-sdk). This also has Typescript types added.

## Installation

### For [React Native](https://react-native.dev/)

The Product Advertising API react-native SDK can be installed via [npm](https://www.npmjs.com/package/paapi5-react-native-sdk):

```shell
npm install paapi5-react-native-sdk --save
```

You should now be able to `require('paapi5-react-native-sdk')` in javascript files.

### For browser

The library also works in the browser environment via npm and [browserify](http://browserify.org/). After following
the above steps with Node.js and installing browserify with `npm install -g browserify`,
perform the following (assuming _main.js_ is your entry file, that's to say your javascript file where you actually
use this library):

```shell
browserify main.js > bundle.js
```

Then include _bundle.js_ in the HTML pages.

### Webpack Configuration

Using Webpack you may encounter the following error: "Module not found: Error:
Cannot resolve module", most certainly you should disable AMD loader. Add/merge
the following section to your webpack config:

```javascript
module: {
  rules: [
    {
      parser: {
        amd: false,
      },
    },
  ];
}
```

## Getting Started

Please follow the [installation](#installation) instruction and execute the following JS code:

Simple example for [SearchItems](https://webservices.amazon.com/paapi5/documentation/search-items.html) to discover Amazon products with the keyword 'Harry Potter' in All categories:

```javascript
var ProductAdvertisingAPIv1 = require("paapi5-react-native-sdk");

var defaultClient = ProductAdvertisingAPIv1.ApiClient.instance;

// Specify your credentials here. These are used to create and sign the request.
defaultClient.accessKey = "<YOUR ACCESS KEY>";
defaultClient.secretKey = "<YOUR SECRET KEY>";

/**
 * Specify Host and Region to which you want to send the request to.
 * For more details refer:
 * https://webservices.amazon.com/paapi5/documentation/common-request-parameters.html#host-and-region
 */
defaultClient.host = "webservices.amazon.com";
defaultClient.region = "us-east-1";

var api = new ProductAdvertisingAPIv1.DefaultApi();

/**
 * The following is a sample request for SearchItems operation.
 * For more information on Product Advertising API 5.0 Operations,
 * refer: https://webservices.amazon.com/paapi5/documentation/operations.html
 */
var searchItemsRequest = new ProductAdvertisingAPIv1.SearchItemsRequest();

/** Enter your partner tag (store/tracking id) and partner type */
searchItemsRequest["PartnerTag"] = "<YOUR PARTNER TAG>";
searchItemsRequest["PartnerType"] = "Associates";

// Specify search keywords
searchItemsRequest["Keywords"] = "Harry Potter";

/**
 * Specify the category in which search request is to be made.
 * For more details, refer:
 * https://webservices.amazon.com/paapi5/documentation/use-cases/organization-of-items-on-amazon/search-index.html
 */
searchItemsRequest["SearchIndex"] = "All";

// Specify the number of items to be returned in search result
searchItemsRequest["ItemCount"] = 1;

/**
 * Choose resources you want from SearchItemsResource enum
 * For more details, refer: https://webservices.amazon.com/paapi5/documentation/search-items.html#resources-parameter
 */
searchItemsRequest["Resources"] = [
  "Images.Primary.Medium",
  "ItemInfo.Title",
  "Offers.Listings.Price",
];

var callback = function (error, data, response) {
  if (error) {
    console.log("Error calling PA-API 5.0!");
    console.log(
      "Printing Full Error Object:\n" + JSON.stringify(error, null, 1)
    );
    console.log("Status Code: " + error["status"]);
    if (
      error["response"] !== undefined &&
      error["response"]["text"] !== undefined
    ) {
      console.log(
        "Error Object: " + JSON.stringify(error["response"]["text"], null, 1)
      );
    }
  } else {
    console.log("API called successfully.");
    var searchItemsResponse =
      ProductAdvertisingAPIv1.SearchItemsResponse.constructFromObject(data);
    console.log(
      "Complete Response: \n" + JSON.stringify(searchItemsResponse, null, 1)
    );
    if (searchItemsResponse["SearchResult"] !== undefined) {
      console.log("Printing First Item Information in SearchResult:");
      var item_0 = searchItemsResponse["SearchResult"]["Items"][0];
      if (item_0 !== undefined) {
        if (item_0["ASIN"] !== undefined) {
          console.log("ASIN: " + item_0["ASIN"]);
        }
        if (item_0["DetailPageURL"] !== undefined) {
          console.log("DetailPageURL: " + item_0["DetailPageURL"]);
        }
        if (
          item_0["ItemInfo"] !== undefined &&
          item_0["ItemInfo"]["Title"] !== undefined &&
          item_0["ItemInfo"]["Title"]["DisplayValue"] !== undefined
        ) {
          console.log("Title: " + item_0["ItemInfo"]["Title"]["DisplayValue"]);
        }
        if (
          item_0["Offers"] !== undefined &&
          item_0["Offers"]["Listings"] !== undefined &&
          item_0["Offers"]["Listings"][0]["Price"] !== undefined &&
          item_0["Offers"]["Listings"][0]["Price"]["DisplayAmount"] !==
            undefined
        ) {
          console.log(
            "Buying Price: " +
              item_0["Offers"]["Listings"][0]["Price"]["DisplayAmount"]
          );
        }
      }
    }
    if (searchItemsResponse["Errors"] !== undefined) {
      console.log("Errors:");
      console.log(
        "Complete Error Response: " +
          JSON.stringify(searchItemsResponse["Errors"], null, 1)
      );
      console.log("Printing 1st Error:");
      var error_0 = searchItemsResponse["Errors"][0];
      console.log("Error Code: " + error_0["Code"]);
      console.log("Error Message: " + error_0["Message"]);
    }
  }
};

try {
  api.searchItems(searchItemsRequest, callback);
} catch (ex) {
  console.log("Exception: " + ex);
}
```

Complete documentation, installation instructions, and examples are available [here](https://webservices.amazon.com/paapi5/documentation/index.html).

## License

This SDK is distributed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0),
see LICENSE.txt and NOTICE.txt for more information.
