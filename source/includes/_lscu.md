# Local SEO Check-up

## Add Report

<span class="label label-info">Account Method</span>

> Add Report

```php
<?php
use BrightLocal\Api;

$api = new Api('<INSERT_API_KEY>', '<INSERT_API_SECRET>');
$success = $api->post('v4/lscu', [
    'report-name'				=> 'Sample SEO Check-Up Report', 
	'business-names'			=> ["Le Bernardin"],
	'website-address'			=> "le-bernardin.com",
	'address1'                  => '155 West 51st Street',
    'address2'                  => '',      
    'city'                      => 'New York',
	'state-code'				=> 'NY',
	'postcode'					=> '10019',	
	'telephone'					=> '+1 212-554-1515',
	'country'					=> 'USA',
	'business-category'         => 'Restaurant',
	'primary-business-location' => 'NY, New York',
	'search-terms'              => ["restaurant manhattan","cafe new york"]
]);
print_r($success);
```

```shell
curl -X POST \
 -d 'api-key=<INSERT_API_KEY>' \
 -d 'sig=<INSERT_API_SIG>' \
 -d 'expires=<INSERT_API_EXPIRES>' \ 
 -d 'report-name=Sample SEO Check-Up Report' \
 -d 'business-names=["Le Bernardin"]' \
 -d 'address1=155 West 51st Street' \ 
 -d 'address2=' \
 -d 'city=New York' \
 -d 'state-code=NY' \ 
 -d 'postcode=10019' \
 -d 'telephone=+1 212-554-1515'
 -d 'country=USA' \
 -d 'business-category=Restaurant' \
 -d 'primary-business-location=NY+NewYork'
 -d 'search-terms=["restaurant manhattan","cafe new york"]'
 https://tools.brightlocal.com/seo-tools/api/v4/lscu
```

> Supplying Local Directory URLs (see local-directory-urls parameter)

```php
<?php
echo json_encode(array(
   'citysearch' => array(
       'url'     => 'http://www.yelp.co.uk/biz/greens-restaurant-san-francisco-3',
       'include' => 'yes'
   ),
   'dexknows'   => array(
       'url'     => ''
       'include' => 'yes'
   ),
   'kudzu'      => array(
       'url'     => '',
       'include' => 'yes'
   )
));
```

> Success (201 Created)

```json
{
    "success": true,
    "report-id": "1"
}
```
 
> Validation Failure (400 Bad Request)

```json
{
  "success": false,
  "errors":  {
    "INVALID_REPORT_NAME": "Report name missing",
    "INVALID_BUSINESS_NAMES": "Business names missing (at least one must be supplied)",
    "INVALID_WEBSITE_ADDRESS": "Website address missing",
    "INVALID_TELEPHONE": "Telephone number missing",
    "INVALID_ADDRESS1": "Address line 1 missing",
    "INVALID_CITY": "City missing",
    "INVALID_COUNTRY": "Country missing",
    "INVALID_STATE_CODE": "State code missing",
    "INVALID_POSTCODE": "Postcode/ZIP missing",
    "INVALID_BUSINESS_CATEGORY": "Business category missing",
    "INVALID_PRIMARY_BUSINESS_LOCATION": "Primary business location missing",
    "INVALID_SEARCH_TERMS": "Search terms missing (please supply at least one)",
    "INVALID_GOOGLE_LOCATION": "Google location missing"
  }
}
```

Adds a new Local SEO Check-up report to your account.

### HTTP Request

`POST https://tools.brightlocal.com/seo-tools/api/v4/lscu`

### Query Parameters

Parameter | Notes
--------- | -----
api-key | <span class="label label-required">Required</span>
sig | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
expires | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
report-name | <span class="label label-required">Required</span>
location-id | Associate this report with a location in your account. This ID needs to correspond to a valid location in your account.
white-label-profile-id | Assign a white label profile to this report. The ID needs to correspond to a valid white label profile in your account.
business-names | <span class="label label-required">Required</span> Supply one or more busines names (max 5) as a JSON string. For example, ["Greens Restaurant","Greens"].
website-address | <span class="label label-required">Required</span> The address for your business website.
address1 | <span class="label label-required">Required</span>
address2 |
area | The neighborhood or district for the business. For example, Charleston or Fulham.
city | <span class="label label-required">Required</span>
state-code | <span class="label label-required">Required</span> (USA, CAN:EN and AUS)
postcode | <span class="label label-required">Required</span> A valid postcode or ZIP.
telephone | <span class="label label-required">Required</span>
country | <span class="label label-required">Required</span> One of USA, CAN:EN, GBR or AUS.
business-category | <span class="label label-required">Required</span> The type of business - for example, Plumber (not Plumbing &amp; Heating Services).
primary-business-location | <span class="label label-required">Required</span> The primary location served by the business. Enter town name and state code - for example, Chicago, IL.
search-terms | <span class="label label-required">Required</span> Supply one or more search terms (max 5) as a JSON string. For example, ["restaurant san francisco","cafe san francisco"].
google-location | A valid google search location. Please refer to our location check method.
bing-location | A valid bing search location. Please refer to our location check method.
notify | One of yes or no. If set to yes we will send report alerts to all email addresses specified (see field below). If you include customer email addresses when setting up your report we'll also email them the alerts so please be sure this is what you want before adding their addresses. Default is no.
email-addresses | Supply one or more (max 5) email addresses for us to send report alerts to. This only takes effect if notify is set to yes.
facebook-url | If known, please supply the Facebook URL for the business. If not supplied we'll try and find it on the business website or through a direct search of Facebook.
twitter-url | If known, please supply the Twitter URL for the business. If not supplied we'll try and find it on the business website or through through a direct search of Twitter.
is-public | Determines whether or not to make the report available on a public URL you can give to your customers. One of yes or no. Defaults to no.
local-directory-urls | <p>By default we try and find local directory profile URLs and information for your business in all directories we support. If you'd like your report to only contain specific directories or already know the profile URLs for some directories you can supply them here. Please note that if you supply information for this parameter you need to give us all details of the directories you want included. If you have profile URLs for some but not others you can leave those URL fields empty and we'll do our best to find them. Generally we recommend you run your report for the first time without this setting and then use our update method to add/change URLs or remove directories if needed.</p><p>The data for this parameter needs to be supplied as a JSON string. Local directory identifiers (the keys in the example below) are documented here.</p>
run-report | One of yes or no. Runs the report after adding. Defaults to no.

## Update Report

<span class="label label-info">Account Method</span>

> Update Report

```php
<?php
use BrightLocal\Api;

$api = new Api('<INSERT_API_KEY>', '<INSERT_API_SECRET>');
$success = $api->put('v4/lscu', [
	'report-id'                 => '1',
	'postcode'                  => '10019',	
	'telephone'                 => '+1 212-554-1515',
	'country'                   => 'USA',
	'business-category'         => 'Restaurant',
	'primary-business-location' => 'NY, New York',
	'search-terms'              => '["restaurant manhattan","cafe new york"]'
]);
print_r($success);
```

```shell
curl -X PUT \
 -d 'api-key=<INSERT_API_KEY>' \
 -d 'sig=<INSERT_API_SIG>' \
 -d 'expires=<INSERT_API_EXPIRES>' \  
 -d 'postcode=10019' \
 -d 'telephone=+1 212-554-1515'
 -d 'country=USA' \
 -d 'business-category=Restaurant' \
 -d 'primary-business-location=NY+NewYork'
 -d 'search-terms=["restaurant manhattan","cafe new york"]'
 https://tools.brightlocal.com/seo-tools/api/v4/lscu
```

> Supplying Local Directory URLs (see local-directory-urls parameter)


```php
<?php
echo json_encode(array(
   'citysearch' => array(
       'url'     => 'http://www.yelp.co.uk/biz/greens-restaurant-san-francisco-3',
       'include' => 'yes'
   ),
   'dexknows'   => array(
       'url'     => ''
       'include' => 'yes'
   ),
   'kudzu'      => array(
       'url'     => '',
       'include' => 'yes'
   )
));
```

> Success (200 OK)

```json
{
    "success": true
}
```
 
> Validation Failure (400 Bad Request)

```json
{
  "success": false,
  "errors":  {
    "INVALID_REPORT_ID": "Report ID missing"
  }
}
```
 
> Failure when report running (400 Bad Request)

```json
{
    "success": false,
    "errors":  {
      "REPORT_RUNNING": "Report is running"
    }
}
```

### HTTP Request

`PUT https://tools.brightlocal.com/seo-tools/api/v4/lscu`

### Query Parameters

Parameter | Notes
--------- | -----
api-key | <span class="label label-required">Required</span>
sig | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
expires | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
report-id | <span class="label label-required">Required</span>
report-name |
location-id | Associate this report with a location in your account. This ID needs to correspond to a valid location in your account.
white-label-profile-id | Assign a white label profile to this report. The ID needs to correspond to a valid white label profile in your account.
business-names | Supply one or more busines names (max 5) as a JSON string. For example, ["Greens Restaurant","Greens"].
website-address | The address for your business website.
address1 |
address2 |
area | The neighborhood or district for the business. For example, Charleston or Fulham.
city |
state-code | (USA, CAN:EN and AUS)
postcode | A valid postcode or ZIP.
telephone |
country | One of USA, CAN:EN, GBR or AUS.
business-category | The type of business - for example, Plumber (not Plumbing &amp; Heating Services).
primary-business-location | The primary location served by the business. Enter town name and state code - for example, Chicago, IL.
search-terms | Supply one or more search terms (max 5) as a JSON string. For example, ["restaurant san francisco","cafe san francisco"].
google-location | A valid google search location. Please refer to our location check method.
bing-location | A valid bing search location. Please refer to our location check method.
notify | One of yes or no. If set to yes we will send report alerts to all email addresses specified (see field below). If you include customer email addresses when setting up your report we'll also email them the alerts so please be sure this is what you want before adding their addresses. Default is no.
email-addresses | Supply one or more (max 5) email addresses for us to send report alerts to. This only takes effect if notify is set to yes.
facebook-url | If known, please supply the Facebook URL for the business. If not supplied we'll try and find it on the business website or through a direct search of Facebook.
twitter-url | If known, please supply the Twitter URL for the business. If not supplied we'll try and find it on the business website or through through a direct search of Twitter.
is-public | Determines whether or not to make the report available on a public URL you can give to your customers. One of yes or no. Defaults to no.
local-directory-urls | <p>This parameter allows you update the profile URLs we have stored for your business on each local directory, to exclude a directory from subsequent report runs or include one that isn't currently present. You can also manually supply profile URLs to correct ones that are incorrect or where we haven't been able to automatically find the relevant profile URL. All changes require the report to be re-run before they take effect.</p><p>The data for this parameter needs to be supplied as a JSON string. Local directory identifiers (the keys in the example below) are documented here. Here's an example of how to generate suitable values in PHP:</p>

## Get Report

<span class="label label-info">Account Method</span>

> Get Report

```php
<?php
use BrightLocal\Api;

$api = new Api('<INSERT_API_KEY>', '<INSERT_API_SECRET>');
$results = $api->get('v4/lscu', [
    'report-id' => 860
]);
print_r($results);
```

```shell
curl -X GET \
 -d 'api-key=<INSERT_API_KEY>' \
 -d 'sig=<INSERT_API_SIG>' \
 -d 'expires=<INSERT_API_EXPIRES>' \
 -d 'report-id=860' \
  https://tools.brightlocal.com/seo-tools/api/v4/lscu
```

> Success (200 OK)

```json
{
  "success": true,
  "report":  {
    "report_id": "860",
    "customer_id": "35",
    "location_id": "0",
    "company_name": null,
    "name": "McCormick & Schmick's",
    "white_label_profile_id": "59",
    "business_names": "McCormick & Schmick's",
    "website_address": "http://www.mccormickandschmicks.com",
    "telephone": "+1 312-923-7226",
    "address_1": "1 E Upper Wacker Dr",
    "address_2": "",
    "area": "",
    "city": "Chicago",
    "state_code": "IL",
    "postcode": "60601",
    "country": "USA",
    "business_category": "restaurant",
    "primary_business_location": "chicago, il",
    "search_terms": "restuarant chicago\r\ncafe chicago",
    "notify": "No",
    "email_addresses": "<hidden>",
    "date_added": "2014-07-29 10:08:41",
    "last_start_time": "2014-07-29 10:10:25",
    "last_end_time": "2014-07-29 10:44:39",
    "last_message": "Report generated successfully",
    "generation_error": "No",
    "currently_running": "No",
    "facebook": "http://www.facebook.com/mccormickandschmicks",
    "twitter": "http://twitter.com/McandSchmicks",
    "google_location": "chicago, il",
    "bing_location": "",
    "previous_bing_location": null,
    "is_public": "Yes",
    "public_key": "<hidden>",
    "latest_run":  {
      "interactive_url": "https://tools.brightlocal.com/seo-tools/admin/lscu/reports/view/858/2171",
      "pdf_url": "https://tools.brightlocal.com/seo-tools/admin/lscu/reports/pdf/858",
      "public_interactive_url": "http://www.local-marketing-reports.com/seo-reports/<hidden>/858",
      "public_pdf_url": "http://www.local-marketing-reports.com/seo-reports/<hidden>/858.pdf"
    }
  }
}
```

> Validation Failure (400 Bad Request)

```json
{
  "success": false,
  "errors":  {
    "INVALID_REPORT_ID" : "Report ID missing"
  }
}
```

### HTTP Request

`GET https://tools.brightlocal.com/seo-tools/api/v4/lscu`

### Query Parameters

Parameter | Notes
--------- | -----
api-key | <span class="label label-required">Required</span>
sig | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
expires | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
report-id | <span class="label label-required">Required</span> The unique ID for the report in your account.

## Run Report

<span class="label label-info">Account Method</span>

> Run Report

```php
<?php
use BrightLocal\Api;

$api = new Api('<INSERT_API_KEY>', '<INSERT_API_SECRET>');
$results = $api->put('v4/lscu/run', [
    'report-id' => 860
]);
print_r($results);
```

```shell
curl -X PUT \
 -d 'api-key=<INSERT_API_KEY>' \
 -d 'sig=<INSERT_API_SIG>' \
 -d 'expires=<INSERT_API_EXPIRES>' \
 -d 'report-id=860' \
  https://tools.brightlocal.com/seo-tools/api/v4/lscu/run
```

> Success (200 OK)

```json
{
    "success": true
}
```
 
> Validation Failure (400 Bad Request)

```json
{
  "success": false,
  "errors":  {
    "INVALID_REPORT_ID" : "Report ID missing",
    "NO_CREDITS" : "You don't have any credits left"
  }
}
```
 
> Failure when report already running (400 Bad Request)

```json
{
    "success": false,
    "errors":  {
      "REPORT_RUNNING": "Report is already running"
    }
}
```

### HTTP Request

`PUT https://tools.brightlocal.com/seo-tools/api/v4/lscu/run`

### Query Parameters

Parameter | Notes
--------- | -----
api-key | <span class="label label-required">Required</span>
sig | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
expires | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
report-id | <span class="label label-required">Required</span> The unique ID for the report in your account.

## Delete Report

<span class="label label-info">Account Method</span>

> Delete Report

```php
<?php
use BrightLocal\Api;

$api = new Api('<INSERT_API_KEY>', '<INSERT_API_SECRET>');
$success = $api->delete('v4/lscu', [
    'report-id' => 860
]);
if($success) {
	echo 'Successfully deleted report.' . PHP_EOL;
}
print_r($success);
```

```shell
curl -X DELETE \
 -d 'api-key=<INSERT_API_KEY>' \
 -d 'sig=<INSERT_API_SIG>' \
 -d 'expires=<INSERT_API_EXPIRES>' \
 -d 'report-id=860' \
  https://tools.brightlocal.com/seo-tools/api/v4/lscu
```

> Success (200 OK)

```json
{
    "success": true
}
```
 
> Validation Failure (400 Bad Request)

```json
{
  "success": false,
  "errors":  {
    "INVALID_REPORT_ID": "Report ID missing"
  }
}
```
 
> Failure when report running (400 Bad Request)

```json
{
    "success": false,
    "errors":  {
      "REPORT_RUNNING": "Report is running"
    }
}
```

### HTTP Request

`DELETE https://tools.brightlocal.com/seo-tools/api/v4/lscu`

### Query Parameters

Parameter | Notes
--------- | -----
api-key | <span class="label label-required">Required</span>
sig | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
expires | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
report-id | <span class="label label-required">Required</span> The unique ID for the report in your account.

## Search

<span class="label label-info">Account Method</span>

> Search for a Report

```php
<?php
use BrightLocal\Api;

$api = new Api('<INSERT_API_KEY>', '<INSERT_API_SECRET>');
$results = $api->get('v4/lscu/search', [
    'q' => 'Bodega Wine Bar'	 
]);
print_r($results);
```

```shell
curl -X GET \
 -d 'api-key=<INSERT_API_KEY>' \
 -d 'sig=<INSERT_API_SIG>' \
 -d 'expires=<INSERT_API_EXPIRES>' \
 -d 'q=Bodega+Wine+Bar' \	
  https://tools.brightlocal.com/seo-tools/api/v4/lscu/search
```

> Success (200 OK)

```json
{
  "success": true,
  "reports":  [
     {
      "report_id": "839",
      "report_name": "Bodega Wine Bar",
      "location_id": "14580",
      "date_added": "2014-01-16 12:50:46",
      "last_run_date": "2014-01-16 12:58:58",
      "last_message": "Report generated successfully",
      "currently_running": "No"
    },
     {
      "report_id": "858",
      "report_name": "Bodega Wine Bar",
      "location_id": "14580",
      "date_added": "2014-06-26 07:46:21",
      "last_run_date": "2014-07-28 16:11:14",
      "last_message": "Report generated successfully",
      "currently_running": "No"
    }
  ]
}
```
 
> Validation Failure (400 Bad Request)

```json
{
  "success": false,
  "errors":  {
    "INVALID_SEARCH": "Search string missing"
  }
}
```

### HTTP Request

`GET https://tools.brightlocal.com/seo-tools/api/v4/lscu/search`

### Query Parameters

Parameter | Notes
--------- | -----
api-key | <span class="label label-required">Required</span>
sig | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
expires | <span class="label label-required">Required</span> [See above for how to generate signature and expires values.](#authentication)
q | Supply an arbitrary search string. We search in location and report names for matches.
