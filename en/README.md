<p align="center">
  <a href="http://moca-tech.net/">
    <img src="https://www.moca-tech.net/logo.png" alt="S2S" height=72>
  </a>
  <h3 align="center">Moca Tech</h3>
  <p align="center">
    S2S API Integration Guide
    <br>
    <a href="https://github.com/moca-tech/Performance-S2S-API" target="_blank">zh</a>
    ·
    en
    <br>
    <a href="http://www.moca-tech.net" target="_blank"><strong>More »</strong></a>
    <br>
    <br>
    <a href="mailto:business@moca-tech.net">Business@moca-tech.net</a>
  </p>






## Note

1. When you check payout, please pay attention to currency, as we support multiply currencies. If your system only supports single currency, please clarify it to your AM during integration. 
2. Regarding daily cap, 0 is defined as open cap. If remaining cap is shown as negative, it implies daily budget is running out. You can pause it and resume again when the budget is reset next day. 
3. Daily cap is reset at 00:00 local time. Please refer time zone which is set. Daily cap is controlled by local time. 

## Request

1. Request URL：http://s2s.moca-tech.net/v2.0/sync
2. Request Method：GET|POST
3. Request Frequency：update every 10 minutes.
4. Remarks：Please pay attention to Key Performance Index(KPI), Timezone and Token(apikey).
5. Request Parameters

| Parameter | Type   | Mandatory | Description                                                  |
| --------- | ------ | --------- | ------------------------------------------------------------ |
| aff_id    | int    | Yes       | Your publisher identification number.<br />e.g. 10001        |
| apikey    | string | Yes       | Request it from your account manager.<br />It can be reset in case you lose it. |
| country   | string | No        | Country targeting<br />e.g. [IN\|ID\|US\|UK\|...\|ALL]<br />ALL: All countries except China |
| state     | string | No        | State targeting (unavailability currently)<br />e.g. Maharashtra |
| city      | string | No        | City targeting (unavailability currently)<br />e.g. Mumbai   |
| device    | string | No        | Device Type<br />e.g. [MOBILE\|PC\|ALL]<br />Default as ALL. |
| traffic   | int    | No        | Traffic type<br />e.g. [0\|1\|2]<br />0: Non Incent 1: Incent 2: Both |
| offer_id  | int    | No        | It is used to get specified campaign details.<br />Other filter conditions are invalid with scepific offer ID. |

6. Request Sample

   > http://s2s.moca-tech.net/v2.0/sync?aff_id=10001&country=ALL&platform=ANDROID&traffic=2&apikey=ABC.. .XYZ

## Response

1. Response: JSON

| Field                                       | Type          | Description                       |
| ------------------------------------------- | ------------- | --------------------------------- |
| result &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | boolean       | [true\|false]                     |
| info                                        | string        | Description of the result.        |
| number                                      | int           | How many campaigns are available. |
| list                                        | array[object] | Campaign list                     |

2. Campaign Information

| Field                                             | Type                                                  | Description |
| ------------------------------------------------------------ | --------------- | ------------------------------------------------------------ |
| offer_id | int | Campaign ID<br /> If a specific campaign ID is not available, it implies the campaign is paused or budget is running out. (Status is synchronized every 10 minutes). |
| offer_name | string | Campaign Name |
| adtype | string | Campaign Type<br />e.g. [APP\|Others] |
| traffic | boolean | Traffic type<br />Incent traffic is allowed or not. |
| countries | string | Country Targeting<br />e.g. IN\|TH\|US<br />Use "\|" to seperate different countries.<br />ALL for global except China. |
| states | string | State Targeting<br />e.g. Maharashtra\|Kerala<br />Use "\|" to seperate different states.<br />Null: no state targeting. |
| cities                                            | string                                                  | City Targeting<br />e.g. Mumbai\|Delhi<br />Use "\|" to seperate different cities.<br />Null: no city targeting. |
| carriers                                     | string                                             | Operator targeting<br />e.g. Airtel<br />ALL or null: no operator targeting. |
| device                                            | string                                                  | Which device type is allowed.<br />e.g. [MOBILE\|PC] |
| platform                                | string                                        | Which operating system is required.<br />e.g. [IOS\|ANDROID] |
| min_version | string | The minimum OS version required.<br />Null: no requirement. |
| quality_description | string | Description of KPI                                           |
| quality_on | string | Quality requirements<br />e.g. [0\|1\|2\|3\|4\|5\|6\|7\|...]<br />0: No KPI requirement<br />1: Register<br />2: Search<br />3: Purchase<br />4: Add to Cart<br />5: Listen Song<br />6: Ask Question<br />7: Video Watch<br />8: Login<br />9: Successful Reply<br />12: Homepage/Open APP<br />17: First Trip<br />18: Load Money<br />21: Subscription<br />25: International flight booking<br />26: Domestic flight booking |
| quality_rate | string | Rate of quality required<br />e.g. 15% |
| quality_type | int | 0: None<br />1: Event Ratio<br />2: Unique Event Ratio<br />91: 1st Day Retention<br />92: 2nd Day Retention<br />97: 7th Day Retention<br />915: 15th Day Retention<br />930: 30th Day Retention |
| paymodel | string | Pricing model<br />e.g. [CPI\|CPA\|CPS\|CPR\|SUB\|DDL\|APK\|PPC] |
| payout | float | Payout<br />e.g. 0.52 |
| currency | string | Currenty Unit<br />e.g. [USD\|CNY\|INR...]<br />If you only support USD, please let your AM known when you integrate the API. |
| time_zone | float | Time zone that daily cap follows.<br />e.g. 5.5 means GMT+5.5hrs. Local time is by India time zone.Daily Cap will be reset at 00:00 at India time. |
| daily_caps | int | Daily cap allocated<br />0: open cap |
| remaining | int | Remaining daily cap.<br />Suggest to control traffic according remaining daily cap and CVR. |
| device_id_mandatory | boolean | GAID/IDFA are mandatory to pass Ture: Client request to pass |
| preview_link | string | Link to preview the product |
| tracking_link | string | Link to Click Through，params as below（All of these can be added in Postback URL）:<br />aff_pub: Your sub-publisher ID or Ad Slot ID<br />aff_sub: Your Click ID which usually be used in your Postback URL.<br />gaid: Google Advertising ID<br />idfa: Apple IDFA<br />app_bundle: Your traffic's App Bundle<br />app_name: Your traffic's App Name<br />Custom Parameters: aff_sub2, aff_sub3, aff_sub4, aff_sub5 |
| link_type | string | Type of tracking link<br />e.g. [WEB\|MARKET] |
| imp_tracker | string | Impression tracking link.<br />Please pass GAID/IDFA if device ID mandatory. |
| rating | float | Recommend start<br />1-5 start, refer to google playstore. |
| package_name | string | Name of app package<br />e.g. com.1234 |
| title | string | Headline of the campaign |
| description | string | Slogan or product description |
| icon | string | Advertising icon |
| creative_list | array[object] | Including URL, format and size |
| creative | string | URL to get creative compressed package. |
| sub_blacklist | array[string] | Traffic from those publishers are requested to pause. |
| sub_whitelist | array[string] | Traffic has good quality and request to scale up. |

3. Response Sample

```json
{
  "result": true,
  "info": "OK",
  "number": 1,
  "list": [
    {
      "min_version": "",
      "device_id_mandatory": true,
      "quality_type": 0,
      "icon": "https://lh3.googleusercontent.com/c0uMGe9aLRQxGtAXB_9xnKnG83VOGRD7NtglvnzPj9SYS3_ZYA_EYhtP86nePn0vjI0=s180-rw",
      "payout": 0.3,
      "rating": 0,
      "description": "We’re changing the way you find and play the music you love. Listen free with a Prime membership or get more with Amazon Music Unlimited.",
      "title": "Prime Music_Af",
      "platform": "ANDROID",
      "sub_whitelist": [ ],
      "states": "",
      "sub_blacklist": [
        "a",
        "b",
        "c"
      ],
      "preview_link": "https://play.google.com/store/apps/details?id=com.amazon.mp3",
      "currency": "USD",
      "quality_rate": "0.00%",
      "tracking_link": "http://tracking.moca-tech.net/go2offer?offer_id=105850&aff_id=10001&bid=0.3",
      "traffic": false,
      "adtype": "APP",
      "cities": "",
      "carriers": "ALL",
      "quality_description": "",
      "countries": "IN",
      "time_zone": 0,
      "creative": "http://s3-ap-southeast-1.amazonaws.com/cactiflos/ad/201908/07d3b5b3b2476aa068eb.zip",
      "offer_id": 10000,
      "offer_name": "10000-Prime Music_Af-CPI-Non Incent-Android(IN)",
      "remaining": 5000,
      "paymodel": "CPI",
      "link_type": "MARKET",
      "quality_on": "",
      "imp_tracker": "",
      "creative_list": [ ],
      "package_name": "com.amazon.mp3",
      "daily_caps": 5000,
      "device": "MOBILE"
    }
  ]
}
```
