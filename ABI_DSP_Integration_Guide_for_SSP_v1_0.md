# ABI DSP Integration Guide for SSP v1.0
***
- [1. ABI RTB Partner?](#1-abi-rtb-partner)
    * [1.1. Introduction](#11-introduction)
    * [1.2. Supported Formats and Requests](#12-supported-formats-and-requests)
        + [1.2.1. ABI RTB Partner Supported Creative Types](#121-abi-rtb-partner-supported-creative-types)
        + [1.2.2. Transmission](#122-transmission)
        + [1.2.3. Headers](#123-headers)
        + [1.2.4. Data Encoding](#124-data-encoding)
- [2. Bid Request](#2-bid-request)
    * [2.1. Bid Request Object](#21-bid-request-object)
    * [2.2. Imp](#22-imp)
    * [2.3. Imp > Banner](#23-imp--banner)
    * [2.4. Imp > Video](#24-imp--video)
    * [2.5. Device](#25-device)
    * [2.6. Device > Geo](#26-device--geo)
    * [2.7. Site](#27-site)
    * [2.8. App](#28-app)
    * [2.9. User](#29-user)
    * [2.10. Imp > PMP](#210-imp--pmp)
    * [2.11. Banner Ad Types](#211-banner-ad-types)
    * [2.12. Ad Position](#212-ad-position)
    * [3. Bid Request Examples](#3-bid-request-examples)
        + [3.1. WEB - BANNER](#31-web---banner)
        + [3.2. APP - BANNER](#32-app---banner)
        + [3.3. APP - VIDEO](#33-app---video)
- [4. Bid Response](#4-bid-response)
    * [4.1. Bid Response](#41-bid-response)
    * [4.2. SeatBid](#42-seatbid)
    * [4.3. SeatBid > Bid](#43-seatbid--bid)
- [5. Bid Response Examples](#5-bid-response-examples)
    * [5.1. RTB - 1](#51-rtb---1)
    * [5.2. RTB - 2](#52-rtb---2)
    * [5.3. PMP](#53-pmp)
    * [5.4. Video](#54-video)
- [6. Macro Types](#6-macro-types)


# 1. ABI RTB Partner?
## 1.1. Introduction
> ABI RTB Partner is an ad auction system that facilitates real-time automated bidding between ad buyers and publisher inventory sellers. It is designed based on OpenRTB Specification version 2.3 ~ 2.6 and Video Ad Serving Template (VAST) 3.0, with only a limited set of Objects implemented rather than the full specification.

## 1.2. Supported Formats and Requests
### 1.2.1. ABI RTB Partner Supported Creative Types
- Display Banner: Returns creatives implemented by ABI in HTML format
- Video: Returns in XML format conforming to the VAST 3.0 specification

### 1.2.2. Transmission
The default protocol is HTTP. POST is used for bid requests. In case of No Bid, HTTP status code 204 is returned; in case of a bid, status code 200 is returned along with the Response Body. Bid requests and responses follow the JSON format.

### 1.2.3. Headers
The HTTP header uses Content-Type to indicate the MIME type. It uses application/json to specify JSON format, and the same header is included in the response.
```
Content-Type: application/json
```

### 1.2.4. Data Encoding
Including the following value in the header allows you to receive data in compressed form.
```
Accept-Encoding: gzip
```
<br><br>

# 2. Bid Request
## 2.1. Bid Request Object
> This is the Object for bid requests. RTB begins by sending a bid request with this Object. The top level contains a unique bid request ID. Required values are id and imp, and either a site or app attribute must be included.

| Attribute | Type          | Required | Comment                                                          |
|-----------|---------------|----------|------------------------------------------------------------------|
| id        | string        | O        | Bid request ID                                                   |
| imp       | array(object) | O        | [See Imp Object table](#22-imp)                                  |
| site      | object        | O (web)  | [See Site Object table](#27-site)                                |
| app       | object        | O (app)  | [See App Object table](#28-app)                                  |
| device    | object        |          | [See Device Object table](#25-device)                            |
| user      | object        |          | [See User Object table](#29-user)                                |
| at        | integer       |          | Auction type (1: 1st price auction, 2: 2nd price auction)        |
| tmax      | integer       |          | Maximum time to respond to bid request (milliseconds)            |
| wseat     | array(string) |          | List of seat IDs allowed to bid on this request                  |
| cur       | array(string) |          | Allowed bid currency units. See ISO-4217<br/>(e.g., 'KRW', 'USD') |
| bcat      | array(string) |          | List of blocked advertiser categories                            |
| badv      | array(string) |          | List of blocked advertiser domains<br/>(e.g., 'aaa.com', 'bbb.com') |

## 2.2. Imp
> Represents the placement and type of the ad being bid on.

| Attribute   | Type          | Required | Comment                                                                                        |
|-------------|---------------|----------|------------------------------------------------------------------------------------------------|
| id          | string        | O        | Unique ID of the ad impression                                                                 |
| banner      | array(object) |          | [See Banner Object table](#23-imp--banner)                                                     |
| video       | object        |          | [See Video Object table](#24-imp--video)                                                       |
| tagid       | object        |          | Identifier for a specific ad placement or ad tag                                               |
| bidfloor    | object        |          | Minimum CPM bid price                                                                          |
| bidfloorcur | object        |          | Currency unit for bidfloor CPM                                                                 |
| secure      | integer       |          | Flag indicating whether HTTPS URL is required for the impression.<br/>0: non-secure (http), 1: secure (https) |

## 2.3. Imp > Banner
> This Object must be included for banner ad requests.

| Attribute | Type           | Required | Comment                                                            |
|-----------|----------------|----------|--------------------------------------------------------------------|
| id        | string         | O        | Unique banner ID                                                   |
| w         | integer        | O        | Banner width                                                       |
| h         | integer        | O        | Banner height                                                      |
| pos       | integer        |          | Ad position code [(See Ad Position table)](#212-ad-position)       |
| btype     | array(integer) |          | Blocked banner type codes [(See Banner Ad Types table)](#211-banner-ad-types) |
| battr     | array(integer) |          | Blocked creative attributes (See IAB OpenRTB 2.3.x)               |


## 2.4. Imp > Video
> This Object must be included for video ad requests.

| Attribute      | Type           | Required | Comment                                               |
|----------------|----------------|----------|-------------------------------------------------------|
| mimes          | array(string)  | O        | Content MIME types                                    |
| minduration    | integer        | O        | Minimum video ad duration (seconds)                   |
| maxduration    | integer        | O        | Maximum video ad duration (seconds)                   |
| protocols      | array(integer) |          | Supported video bid response protocols                |
| w              | integer        | O        | Video player width (pixels)                           |
| h              | integer        | O        | Video player height (pixels)                          |
| linearity      | integer        |          | Whether the impression is linear or non-linear        |
| startdelay     | integer        |          | Start delay of the ad in seconds                      |
| placement      | integer        |          | Placement type of the impression                      |
| sequence       | integer        |          | Sequence number for multiple impressions              |
| playbackmethod | array(integer) |          | See IAB OpenRTB Spec 2.5 > 3.2.7 table               |
| minbitrate     | integer        |          | Minimum bit rate                                      |
| maxbitrate     | integer        |          | Maximum bit rate                                      |
| delivery       | array(integer) |          | Supported delivery methods. See IAB OpenRTB Spec 2.5 > 5.11 table |


## 2.5. Device
> Contains information related to the device interacting with the user.

| Attribute  | Type    | Required | Comment                                                  |
|------------|---------|----------|----------------------------------------------------------|
| ua         | string  |          | Browser User Agent                                       |
| dnt        | integer |          | Do Not Track<br/>0: tracking allowed, 1: tracking denied |
| ip         | string  |          | Device IP Address                                        |
| devicetype | integer |          | Device type                                              |
| make       | string  |          | Device manufacturer                                      |
| model      | string  |          | Device model                                             |
| os         | string  |          | Device OS                                                |
| osv        | string  |          | Device OS Version                                        |
| geo        | object  |          | Device location information                              |
| ifa        | string  |          | Ad identifier ID (type: idfa, gaid, etc.)                |
| didsha1    | string  |          | SHA1-hashed Device ID                                    |
| didmd5     | string  |          | MD5-hashed Device ID                                     |
| dpidsha1   | string  |          | SHA1-hashed Platform Device ID                           |
| dpidmd5    | string  |          | MD5-hashed Platform Device ID                            |

## 2.6. Device > Geo
> Object representing the location information of the device.

| Attribute | Type    | Required | Comment              |
|-----------|---------|----------|----------------------|
| lat       | float   |          | Latitude             |
| lon       | float   |          | Longitude            |
| type      | integer |          | Location source type |
| country   | string  |          | Country code         |
| region    | string  |          | Region code          |

## 2.7. Site
> This Object must be included when the ad delivery surface is a website. A bid request cannot contain both Site and App objects.

| Attribute | Type          | Required       | Comment                                         |
|-----------|---------------|----------------|-------------------------------------------------|
| id        | string        | O (if web)     | Exchange Site ID                                |
| name      | string        |                | Exchange Site Name                              |
| domain    | string        |                | Publisher domain (e.g., 'www.performancebytbwa.net') |
| cat       | array(string) |                | IAB content category list for the site          |
| page      | string        |                | URL of the current page                         |

## 2.8. App
> This Object must be included when the ad delivery surface is a non-browser format such as mobile. A bid request cannot contain both Site and App objects.

| Attribute | Type          | Required       | Comment                       |
|-----------|---------------|----------------|-------------------------------|
| id        | string        | O (if app)     | Exchange App ID               |
| name      | string        |                | Exchange App Name             |
| bundle    | string        |                | App Bundle or Package name    |
| cat       | array(string) |                | Content category list for app |

## 2.9. User
> Represents device user information.

| Attribute | Type          | Required | Comment                    |
|-----------|---------------|----------|----------------------------|
| id        | string        |          | Exchange unique user ID    |
| buyeruid  | string        |          | Exchange App Name          |

## 2.10. Imp > PMP
> Contains information about Private Marketplace (PMP) deals. Defines pre-agreed deal terms between specific buyers and sellers.

| Attribute       | Type          | Required | Comment                                                                              |
|-----------------|---------------|----------|--------------------------------------------------------------------------------------|
| private_auction | integer       |          | Private auction flag. 0: all bids allowed, 1: only seats specified in deals may bid  |
| deals           | array(object) |          | List of Deal Objects. See Deal Object table below                                    |

### 2.10.1. Imp > PMP > Deal
> Represents individual deal information within a PMP transaction.

| Attribute   | Type          | Required | Comment                                                                |
|-------------|---------------|----------|------------------------------------------------------------------------|
| id          | string        | O        | Unique Deal ID                                                         |
| bidfloor    | float         |          | Minimum CPM bid price for the deal                                     |
| bidfloorcur | string        |          | Currency unit for bidfloor. See ISO-4217 (e.g., 'USD', 'KRW')         |
| at          | integer       |          | Auction type (1: 1st price, 2: 2nd price, 3: fixed price per deal)    |
| wseat       | array(string) |          | List of seat IDs allowed to bid on this deal                           |
| wadomain    | array(string) |          | List of advertiser domains allowed to bid on this deal                 |

## 2.11. Banner Ad Types
> The following table describes banner ad types.

| Attribute | Comment              |
|-----------|----------------------|
| 1         | XHTML Text Ad        |
| 2         | XHTML Banner Ad      |
| 3         | Javascript Ad        |
| 4         | iframe Ad            |

## 2.12. Ad Position
> The following table describes ad position attributes.

| Attribute | Comment                                                               |
|-----------|-----------------------------------------------------------------------|
| 0         | Unknown                                                               |
| 1         | Above the Fold                                                        |
| 2         | DEPRECATED - May or may not be initially visible depending on screen size/resolution |
| 3         | Below the Fold                                                        |
| 4         | Header                                                                |
| 5         | Footer                                                                |
| 6         | Sidebar                                                               |
| 7         | Full screen                                                           |

<br><br>

## 3. Bid Request Examples
### 3.1. WEB - BANNER
```json
{
  "id":"4321h3245g1234y1234iu2345k63456lkj2345j4",
  "at":1,
  "cur":["USD"],
  "imp":
  [
    {
      "id":"1",
      "bidfloor":0.03,
      "bidfloorcur":"USD",
      "banner":
      {
        "h":200,
        "w":300,
        "pos":0
      }
    }
  ],
  "site":
  {
    "id":"123456",
    "cat":["IAB5-1","IAB5-2","IAB5-3"],
    "domain":" www.test.com ",
    "page":"http://www.test.com/test.html"
  },
  "device":
  {
    "ua":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36",
    "ip":"172.217.31.132"
  },
  "user":
  {
    "id":"1234b2345m3456j2345g2345g2345ui2345u245a"
  },
  "source":
  {
    "tid":"1234b2345m3456j2345g2345g2345ui2345u245a"
  }
}
```

### 3.2. APP - BANNER
```json
{
    "id": "b680210a-17b4-4650-8bd9-f99c89c74e3a",
    "imp": [
        {
            "id": "1eb3fd3b-b52e-4f33-8474-3762efdcb64d",
            "instl": 0,
            "tagid": "f9278757",
            "secure": 1,
            "banner": {
                "w": 320,
                "h": 480,
                "btype": [
                ],
                "pos": 0
            },
            "bidfloor": 0.0809688576,
            "bidfloorcur": "USD"
        }
    ],
    "device": {
        "carrier": "LG Uplus",
        "ip": "218.39.94.47",
        "language": "ko",
        "make": "LG",
        "model": "LGM-G600S",
        "os": "Android",
        "osv": "7.0",
        "devicetype": 4,
        "ifa": "894dfcb3-8ac4-43c0-a69c-012345678901",
        "geo": {
            "lat": 37.5985,
            "lon": 126.9783,
            "type": 2,
            "city": "Seoul",
            "zip": "02878",
            "country": "KOR",
            "region": "11"
        }
    },
    "app": {
        "id": "1233434",
        "name": "AppName",
        "bundle": "com. app",
        "storeurl": "https://play.google.com/store/apps/details?id=com.app",
        "cat": ["IAB3"],
        "publisher": {
            "name": "Publisher Inc.",
            "id": "a345434e-31ba-488c-939c-290c48d577e4"
        }
    },
    "cur": ["USD"],
    "tmax": 234,
    "badv": ["block.com", "adv_test.com"],
    "at": 2
}
```

### 3.3. APP - VIDEO
```json
{
  "id": "flower.req.id-b8cc05be-adec-4472-8d7a-0dd961015310",
  "imp": [
    {
      "id": "1",
      "video": {
        "mimes": ["video/mp4"],
        "minduration": 5,
        "maxduration": 1000,
        "protocols": [2, 3, 5, 6],
        "w": 1920,
        "h": 1080,
        "startdelay": 0,
        "linearity": 1
      },
      "instl": 1,
      "tagid": "apm_linear_q_skbroadband_01",
      "bidfloor": 3,
      "bidfloorcur": "USD",
      "secure": 0,
      "pmp": {
        "private_auction": 0,
        "deals": [
          {
            "id": "d227e695-3aa2-4683-ab4c-e8c392a7c70c",
            "bidfloor": 3,
            "bidfloorcur": "USD",
            "at": 3
          }
        ]
      }
    }
  ],
  "app": {
    "id": "tv.skb",
    "bundle": "tv.skb",
    "domain": "skbrodband.com",
    "publisher": {
      "id": "1"
    }
  },
  "device": {
    "ua": "SupplySide-SSP/2.0",
    "geo": {
      "lat": 37.56,
      "lon": 126.91,
      "region": "KR-11",
      "city": "Seoul",
      "country": "KOR",
      "zip": "23145"
    },
    "ip": "/127.0.0.1:37288",
    "devicetype": 3,
    "language": "ko",
    "ifa": "test_ad"
  },
  "at": 1,
  "tmax": 500
}
```

<br><br>

# 4. Bid Response
## 4.1. Bid Response
> One response is returned for each bid request. The top level of the Object uses the same ID from the Bid Request.

| Attribute | Type          | Required | Comment                                          |
|-----------|---------------|----------|--------------------------------------------------|
| id        | string        | O        | BidRequest ID                                    |
| seatid    | array(object) |          | See SeatBid table                                |
| bidid     | string        |          | Response ID generated by the bidder for logging/tracking |
| cur       | string        |          | Currency of the bid price                        |
| nbr       | integer       |          | No-bid reason code                               |

## 4.2. SeatBid
| Attribute | Type          | Required | Comment              |
|-----------|---------------|----------|----------------------|
| bid       | array(object) |          | See Bid Object table |
| seatid    | string        |          | Bidder seat ID       |

## 4.3. SeatBid > Bid
| Attribute | Type           | Required | Comment                                                  |
|-----------|----------------|----------|----------------------------------------------------------|
| id        | string         | O        | Unique ID generated by the bidder for logging/tracking   |
| impid     | string         | O        | ID of the Imp item from BidRequest                       |
| price     | float          | O        | Bid price in CPM                                         |
| adid      | string         |          | Ad ID to be served upon winning                          |
| nurl      | string         |          | Win notice Postback URL to notify the bidder upon winning |
| lurl      | string         |          | Loss notice URL to notify the bidder upon losing         |
| adm       | string         | O        | Ad markup to be served upon winning                      |
| adomain   | array(string)  | O        | Advertiser domain list for block list verification       |
| bundle    | object         |          | Advertiser app Bundle ID or package name                 |
| iurl      | string         | O        | Image URL                                                |
| cid       | string         |          | Campaign ID                                              |
| crid      | string         |          | Creative ID                                              |
| cat       | array(string)  |          | IAB category list for the creative (See IAB OpenRTB 2.3.x) |
| attr      | array(integer) |          | Creative attribute list (See IAB OpenRTB 2.3.x)         |

<br><br>

# 5. Bid Response Examples
## 5.1. RTB - 1
```json
{
    "id": "ptbwa_593c7ed4-1650-4304-aeed-02560b41e84c",
    "cur": "USD",
    "seatbid": [
        {
            "seat": "151",
            "bid": [
                {
                    "id": "b680210a-17b4-4650-8bd9-f99c89c74e3a",
                    "impid": "1eb3fd3b-b52e-4f33-8474-3762efdcb64d",
                    "price": 0.0827501724672,
                    "nurl": "https://bid.exemple.com/ptbwa/v1/rtb.nurl?tid=ptbwa_593c7ed4-1650-4304-aeed-02560b41e84c&is_cpvc=0&imp_id=1eb3fd3b-b52e-4f33-8474-3762efdcb64d&imp_idx=0",
                    "iurl": "https://creative.exemple.com/creatives/image/2024/03/19/14/20240319140509002.jpg",
                    "lurl": "https://bid.exemple.com/ptbwa/v1/rtb.lurl?tid=ptbwa_593c7ed4-1650-4304-aeed-02560b41e84c&is_cpvc=0&imp_id=1eb3fd3b-b52e-4f33-8474-3762efdcb64d&imp_idx=0",
                    "adm": "<div style=\"width:320px; height:480px; z-index:999; overflow:hidden; margin-left:auto; margin-right:auto;\" onclick=\"new Image().src='https://bid.exemple.com/ptbwa/v1/rtb.click?tid=ptbwa_593c7ed4-1650-4304-aeed-02560b41e84c&is_cpvc=0&imp_id=1eb3fd3b-b52e-4f33-8474-3762efdcb64d&imp_idx=0';\"><meta name='viewport' content='width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no' /><style type='text/css'>body {margin: 0;padding: 0;}</style><a href=\"https://migunlife.co.kr/%EB%AF%B8%EA%B1%B4%EC%B2%B4%ED%97%98%EC%84%BC%ED%84%B0-%EB%AA%A8%EC%A7%91\" target=\"_blank\"><img width=\"320\" height=\"480\" src=\"https://creative.exemple.com/creatives/image/2024/03/19/14/20240319140509002.jpg\" /></a></div><img src=\"https://bid.exemple.com/ptbwa/v1/rtb.imp?tid=ptbwa_593c7ed4-1650-4304-aeed-02560b41e84c&auction_price=${AUCTION_PRICE}&is_cpvc=1&imp_id=1eb3fd3b-b52e-4f33-8474-3762efdcb64d&imp_idx=0\" style=\"width:0px; height:0px; border:0px;\">",
                    "adomain": ["advertiserdomain.com"],
                    "bundle": "",
                    "cat": ["IAB01"],
                    "cid": "151_272",
                    "crid": "122",
                    "attr": [],
                    "seat": "nhn"
                }
            ]
        }
    ]
}
```

## 5.2. RTB - 2
```json
{
    "id": " ptbwa_8201c812-f432-4aab-ab59-39bbd640e7d5",
    "bidid": "a1123",
    "cur": "USD",
    "seatbid": 
    [
        {
            "seat": "123",
            "bid": 
            [
                {
                    "id": "1",
                    "impid": "102",
                    "price": 9.43,
                    "nurl": "http://adserver.com/winnotice?impid=102",
                    "lurl": "http://adserver.com/lossnotice?impid=102&lossreason=${AUCTION_LOSS}",
                    "adm": "<!DOCTYPE html><html>…$(AUCTION_PRICE}</html>",
                    "iurl": "http://adserver.com/pathtosampleimage",
                    "adomain": [ "advertiserdomain.com" ],
                    "cid": "campaign1",
                    "crid": "creative1",
                    "attr": [ 4, 5, 6, 7 ]
                }
            ]
        }
    ]
}
```

## 5.3. PMP
```json
{
  "id": "ptbwa_8201c812-f432-4aab-ab59-39bbd640e7d3",
  "cur": "USD",
  "seatbid": [
    {
      "seat": "82",
      "bid": [
        {
          "id": "flower.req.id-b8cc05be-adec-4472-8d7a-0dd961015310",
          "impid": "1",
          "price": 3.0831,
          "nurl": "",
          "iurl": "https://creative.exemple.com/creatives/video/2023/12/13/15/20231213153604001.mp4",
          "lurl": "",
          "adm": "<VAST version=\"3.0\"><Ad id=\"73\">........</InLine></Ad></VAST>",
          "adomain": ["adomain.com"],
          "bundle": "",
          "cat": [],
          "dealid": "d227e695-3aa2-4683-ab4c-e8c392a7c70c",
          "dealbidfloor": 3,
          "dealbidfloorcur": "USD",
          "cid": "82_215",
          "crid": "73",
          "attr": [],
          "seat": "anypointmedia"
        }
      ]
    }
  ]
}
```

## 5.4. Video
> For video ad requests, the response returns XML conforming to the VAST 3.0 specification.
```xml
<VAST version="3.0">
    <Ad id="125">
        <InLine>
            <AdSystem version="1.0">
                <![CDATA[ABI]]>
            </AdSystem>
            <AdTitle>
                <![CDATA[Campaign Title]]>
            </AdTitle>
            <Description>
                <![CDATA[Campaign Description]]>
            </Description>
            <e>
                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.error?tag=KLT8FS4IEW2X&req_id=uuid&code=${CODE}]]>
            </e>
            <Impression id="0">
                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.imp?tag=KLT8FS4IEW2X&req_id=uuid&res_id=ptbwa_cc6c5bb3-58c1-486e-a735-882e28168676&tag_id=vod&cmp=146&ag=265&creative=125&auction_price=${AUCTION_PRICE}&ad_type=3&app_id=&app_name=gima&bundle=google.ima&ifa=53bc7daf-4e0f-4274-a799-022a6e46dfa6]]>
            </Impression>
            <Creatives>
                <Creative id="125" sequence="1">
                    <Linear>
                        <Duration>
                            <![CDATA[00:00:14]]>
                        </Duration>
                        <TrackingEvents>
                            <Tracking event="start">
                                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.vast/tracking/start?tag=KLT8FS4IEW2X&req_id=uuid&res_id=ptbwa_cc6c5bb3-58c1-486e-a735-882e28168676&tag_id=vod&cmp=146&ag=265&creative=125&auction_price=${AUCTION_PRICE}&ad_type=3&app_id=&app_name=gima&bundle=google.ima&ifa=53bc7daf-4e0f-4274-a799-022a6e46dfa6]]>
                            </Tracking>
                            <Tracking event="firstQuartile">
                                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.vast/tracking/firstQ?tag=KLT8FS4IEW2X&req_id=uuid&res_id=ptbwa_cc6c5bb3-58c1-486e-a735-882e28168676&tag_id=vod&cmp=146&ag=265&creative=125&auction_price=${AUCTION_PRICE}&ad_type=3&app_id=&app_name=gima&bundle=google.ima&ifa=53bc7daf-4e0f-4274-a799-022a6e46dfa6]]>
                            </Tracking>
                            <Tracking event="midpoint">
                                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.vast/tracking/mid?tag=KLT8FS4IEW2X&req_id=uuid&res_id=ptbwa_cc6c5bb3-58c1-486e-a735-882e28168676&tag_id=vod&cmp=146&ag=265&creative=125&auction_price=${AUCTION_PRICE}&ad_type=3&app_id=&app_name=gima&bundle=google.ima&ifa=53bc7daf-4e0f-4274-a799-022a6e46dfa6]]>
                            </Tracking>
                           <Tracking event="thirdQuartile">
                                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.vast/tracking/thirdQ?tag=KLT8FS4IEW2X&req_id=uuid&res_id=ptbwa_cc6c5bb3-58c1-486e-a735-882e28168676&tag_id=vod&cmp=146&ag=265&creative=125&auction_price=0.5&ad_type=3&app_id=&app_name=gima&bundle=google.ima&ifa=53bc7daf-4e0f-4274-a799-022a6e46dfa6]]>
                            </Tracking>
                            <Tracking event="complete">
                                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.vast/tracking/complete?tag=KLT8FS4IEW2X&req_id=uuid&res_id=ptbwa_cc6c5bb3-58c1-486e-a735-882e28168676&tag_id=vod&cmp=146&ag=265&creative=125&auction_price=0.5&ad_type=3&app_id=&app_name=gima&bundle=google.ima&ifa=53bc7daf-4e0f-4274-a799-022a6e46dfa6]]>
                            </Tracking>
                            <Tracking event="progress" offset="00:00:10">
                                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.vast/tracking/progress?tag=KLT8FS4IEW2X&req_id=uuid&res_id=ptbwa_cc6c5bb3-58c1-486e-a735-882e28168676&tag_id=vod&cmp=146&ag=265&creative=125&auction_price=0.5&ad_type=3&app_id=&app_name=gima&bundle=google.ima&ifa=53bc7daf-4e0f-4274-a799-022a6e46dfa6]]>
                            </Tracking>
                        </TrackingEvents>
                        <VideoClicks>
                            <ClickTracking id="blog">
                                <![CDATA[https://bid.exemple.com/ptbwa/v1/rtb.click?tag=KLT8FS4IEW2X&req_id=uuid&res_id=ptbwa_cc6c5bb3-58c1-486e-a735-882e28168676&tag_id=vod&cmp=146&ag=265&creative=125&auction_price=0.5&ad_type=3&app_id=&app_name=gima&bundle=google.ima&ifa=53bc7daf-4e0f-4274-a799-022a6e46dfa6]]>
                            </ClickTracking>
                        </VideoClicks>
                        <MediaFiles>
                            <MediaFile id="125" delivery="progressive" type="video/mp4" bitrate="28351" width="1920" height="1080">
                                <![CDATA[https://creative.exemple.com/creatives/video/2024/05/16/15/20240516153526001.mp4]]>
                            </MediaFile>
                        </MediaFiles>
                    </Linear>
                </Creative>
            </Creatives>
        </InLine>
    </Ad>
</VAST>
```

<br><br>

# 6. Macro Types
> The Win Notice URL is determined by the bidder. Several macros may be inserted into the URL to convey information such as price to the bidder. Upon winning, the publisher replaces the macros with appropriate values. If there is no value to substitute, the macro is removed from the URL.

| Macro               | Comment                                                            |
|---------------------|--------------------------------------------------------------------|
| ${AUCTION_ID}       | Bid request ID. Value of BidRequest.id                             |
| ${AUCTION_BID_ID}   | Bid ID. Value of BidResponse.bidid                                 |
| ${AUCTION_IMP_ID}   | Impression ID. Value of BidResponse.seatbid.bid.impid              |
| ${AUCTION_SEAT_ID}  | Seat ID. Value of BidResponse.seatbid.seat                        |
| ${AUCTION_AD_ID}    | Ad markup ID the bidder intends to serve. Value of BidResponse.seatbid.bid.adid |
| ${AUCTION_PRICE}    | Settlement price in the same currency and units as the bid         |
| ${AUCTION_CURRENCY} | Currency used in the bid. For verification only                    |

<br>
