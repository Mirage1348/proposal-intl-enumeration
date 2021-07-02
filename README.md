# Intl Enumeration API Specification
List supported values of options in pre-existing ECMA 402 API.

## Stage 2
* [Advanced to Stage 1 in TC39 June 2020 meeting](https://docs.google.com/presentation/d/17bkiVWuYxhMc24If72d6oENK3G6G-irO2EB4EEQCgxU). 
* [Advanced to Stage 2 in TC39 September 2020](https://docs.google.com/presentation/d/1IWOHZVkXL_qbjz4T76bXmtLh7VOrWkT-HJIH2ZwY6NU) meeting.
* [Update in TC39 Nov 2020](https://docs.google.com/presentation/d/1t0P8SKpcU9K_h-jYQ6ix5-02_mF1sAKt6v9AzipX3pw) meeting.
* [Update in TC39 Apr 2021](https://docs.google.com/presentation/d/1LLuJJvGsppQfFf0eCBBcplub_E7NY4EdbSVeE2duyoA) meeting.
* Plan to propose for Stage 3 advancement in TC39 May 2021 meeting

### Entrance Criteria for Stage 1 (Proposal)
* Identified “champion” who will advance the addition: **Frank Yung-Fong Tang**
* Prose outlining the problem or need and the general shape of a solution: **See this document**
* Illustrative examples of usage: **See this document**
* High-level API: **See this document**
* Discussion of key algorithms, abstractions and semantics
* Identification of potential “cross-cutting” concerns and implementation challenges/complexity
* A publicly available repository for the proposal that captures the above requirements: [**https://github.com/tc39/proposal-intl-enumeration**](https://github.com/tc39/proposal-intl-enumeration)
### Entrance Criteria for Stage 2 (Draft)
* Above
* Initial spec text: [**https://tc39.es/proposal-intl-enumeration**](https://tc39.es/proposal-intl-enumeration)
* **Acceptance Signifies**: The committee expects the feature to be developed and eventually included in the standard

### Entrance Criteria for Stage 3 (Candidate)
* Above
* Complete spec text
* Designated reviewers have signed off on the current spec text
* All ECMAScript editors have signed off on the current spec text
* **Acceptance Signifies**: The solution is complete and no further work is possible without implementation experience, significant usage and external feedback.

## Motivation

ECMA402 allows the set of supported local time zones, collations, calendars, numbering systems and  currency as implementation dependents. This proposal provides an API to identify the supported values of these options in the implementation. 

The proposed API empower the caller to feature detect the support in the implementation easily and could be used to download polyfill for missing support easily in the beginning of the loading time. For example, a web page could exam the return value and decide it need to import and install a DateTimeFormat polyfill because the return time zones does not contains the server stored user time zone preference and will not be able to use Intl.DateTimeFormat to format the time according to the server side stored time zone preferences. Users may set up their account and store their time zone preferences in the server side while previously using a different user agent which already supports such a time zone and need to use the user agent in another machine in an internet cafe to access the web application. The proposed API allows the web application to do an up front check and import polyfill from server side while the prefered time zone is not available in the user agent. Similarly, the web application may check for the supported set of currency and unit to implement the bootstrap logic of dynamic import polyfill for some javascript library. For example, engineers in Google are currently changing the closure library implementation to use ECMA402 support if the functionality is available to avoid unnecessary data download, but still download the necessary data and import polyfill while the desired support is not available. The proposed API allows the library to determine the logic with a very minimum amount of data.

Web applications can also combine the use of the proposed API with Intl.DisplayNames and Intl.NumberFormat to build a powerful UI without downloading a lot of data. For example, web applications can use the proposed API to get the list of supported currency, then use Intl.DisplayNames to get back the display name of the currency to build a menu for user to select the calendar, and format the currency amount by using Intl.NumberFormat API. The return value could also support the code to decide which set of the currency conversion information should be downloaded from the server and only download the set of currency the implementation knows how to format correctly, or download all the currency tables and then import polifil to support the one which is not supported. Without the proposed API the web application may either download an extra currency exchange table for currency which won’t be formatted correctly by Intl.NumberFormat and neither be able to efficiently determine which set of currency format data need to be imported with the polyfill. The proposed API empowers the web developer to design an efficient system to import minimum information from the server to implement such a fallback mechanism. 

In summary, the proposed API is an important facility to allow web developers to detect missing support and import fallback polyfills efficiently with a minimum amount of data in the application initialization time. It also allows web application, in conjunction with using other ECMA402 API to program powerful internationalization UI with very minimum information from server side. 


## Overview

One method of Intl, return iteratable based on options

```javascript
Intl.supportedValuesOf(key, [options])
```

## Background

https://github.com/tc39/ecma402/issues/435

## Use case
Feature Tests for newly added values in options supported in Intl API.
For example, web developer may want to use Chinese calendar if avaiable, or ROC calendar as a fallback if avaiable, otherwise Gregorian calendar as final resort. This API allow the code to check which calendar are avaiable and therefor to decide the fallback logic such as importing polyfill from the server.

## Prior Arts
### Get the List of TimeZone

* Use Cases:
  * [How to get list of all timezones in javascript on Stack Overflow](https://stackoverflow.com/questions/38399465/how-to-get-list-of-all-timezones-in-javascript)
* Prior Arts:
  * [momentjs moment.tz.names](https://momentjs.com/timezone/docs/#/data-loading/getting-zone-names/)
  * [tzdb](https://github.com/vvo/tzdb/)
  * [countries-and-timezones](https://www.npmjs.com/package/countries-and-timezones)
  * [country-timezone](https://www.npmjs.com/package/country-timezone)

### Get the List of Currency Codes

* Use Cases:
  * 
* Prior Arts:
  * [currency-codes](https://www.npmjs.com/package/currency-codes)


## Usage example


```javascript
// Find out the supported calendars
Intl.supportedValuesOf("calendar").forEach(function(calendar) {
   // 'buddhist', 'chinese',  ... 'islamicc'
});

// Find out the supported currencies
Intl.supportedValuesOf("currency").forEach(function(currency) {
   // 'AED', 'AFN', 'ALL', ... 'ZWL'
});

// Find out the supported numbering systems
Intl.supportedValuesOf("numberingSystem").forEach(function(nu) {
  // 'adlm', 'ahom', 'arab', ...  'wara', 'wcho'
});

// Find out the supported time zones
Intl.supportedValuesOf("timeZone").forEach(function(timeZone) {
  // 'Africa/Abidjan', 'Africa/Accra', ... 'Pacific/Wallis'
});

// Find out the supported units
Intl.supportedValuesOf("unit").forEach(function(unit) {
  // 'acre', 'bit', 'byte', ... 'year'
});

```


## Authors
* Frank Tang (@FrankYFTang)

## Designated reviewers
* Shane Carr @sffc
* Jordan Harband @ljharb
## ECMAScript editors
* Richard Gibson @gibson042

## Proposal

### Spec
* https://tc39.es/proposal-intl-enumeration/

## References
### Analysis of Privacy / Fingerprinting Concerns
* [“Intl.Enumeration Privacy Implications Mozilla’s Recommendation”.](https://docs.google.com/document/d/1Zw6cYNJpL69HtQfA4-S7bKlCPywhhmoF6Mja-qy-JpU), Tom Ritter, Anne van Kesteren, Steven Englehardt, Zibi Braniecki, Mozilla, Feb 4, 2021.
* [Discussion during 2021-03 ECMA402 Meeting](https://github.com/tc39/ecma402/blob/master/meetings/notes-2021-03-11.md#privacy-evaluation-of-the-api-3)
* Duscussion during 2021-04 ECMA402 Meeting (link to be added soon)

## Implementation Status
* [v8 tracking bug](https://bugs.chromium.org/p/v8/issues/detail?id=10743)
* Mozilla
* JSC
* Test262




============================ Ignore Text Below ============================


#### The following are from the template, will be deleted later 

This repo is setup by following instruction on [TC39 template-for-proposals](https://github.com/tc39/template-for-proposals)

      Your explainer can point readers to the `index.html` generated from `spec.emu`
      via markdown like

      ```markdown
      You can browse the [ecmarkup output](https://ACCOUNT.github.io/PROJECT/)
      or browse the [source](https://github.com/ACCOUNT/PROJECT/blob/master/spec.emu).
      ```

      where *ACCOUNT* and *PROJECT* are the first two path elements in your project's Github URL.
      For example, for github.com/**tc39**/**template-for-proposals**, *ACCOUNT* is "tc39"
      and *PROJECT* is "template-for-proposals".


## Maintain your proposal repo

  1. Make your changes to `spec.emu` (ecmarkup uses HTML syntax, but is not HTML, so I strongly suggest not naming it ".html")
  1. Any commit that makes meaningful changes to the spec, should run `npm run build` and commit the resulting output.
  1. Whenever you update `ecmarkup`, run `npm run build` and commit any changes that come from that dependency.
  
  [explainer]: https://github.com/tc39/how-we-work/blob/master/explainer.md
