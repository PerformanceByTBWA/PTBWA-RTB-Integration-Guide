# Addirect Integration Guide for Airbridge v1.0

---

- [1. Overview](#1-overview)
- [2. Creating a Tracking Link](#2-creating-a-tracking-link)
  - [2.1. Navigating to the Tracking Link Menu](#21-navigating-to-the-tracking-link-menu)
  - [2.2. Selecting the Channel and Configuring Optimization Parameters](#22-selecting-the-channel-and-configuring-optimization-parameters)
- [3. Verifying and Copying the Tracking Link](#3-verifying-and-copying-the-tracking-link)
  - [3.1. Link Creation Complete – Overview of Items](#31-link-creation-complete--overview-of-items)
  - [3.2. Selecting and Copying the CTV Tracking Link](#32-selecting-and-copying-the-ctv-tracking-link)
- [4. Registering on the Addirect App Integration Management](#4-registering-on-the-addirect-app-integration-management)
- [5. Configuring Postbacks in Airbridge](#5-configuring-postbacks-in-airbridge)
  - [5.1. Navigating to the Postback Settings Menu](#51-navigating-to-the-postback-settings-menu)
  - [5.2. Enabling Postback Sending and Selecting Events](#52-enabling-postback-sending-and-selecting-events)
  - [5.3. Configuring Event Data and Delivery Rules](#53-configuring-event-data-and-delivery-rules)
  - [5.4. Applying and Verifying the Postback Settings](#54-applying-and-verifying-the-postback-settings)
- [6. References](#6-references)

---

<br><br>

## 1. Overview

> This document explains how advertisers can create a tracking link for Addirect on the Airbridge dashboard and register it in the Addirect service to complete the integration.

<br><br>

## 2. Creating a Tracking Link

### 2.1. Navigating to the Tracking Link Menu

After logging into the Airbridge dashboard, click **`트래킹 링크 (Tracking Links) > 트래킹 링크 생성 (Create Tracking Link)`** from the left-side menu.

### 2.2. Selecting the Channel and Configuring Optimization Parameters

1. On the tracking link creation page, make sure the **`연동 채널 (Integrated Channel)`** tab is selected at the top.
2. In the **캠페인 최적화 파라미터 (Campaign Optimization Parameters) > 광고주 (Advertiser)** section, click the `채널 (utm_source) * 필수 (Required)` input field.
3. Search for and select **`프라핏DSP`** from the dropdown list.

![Airbridge Channel Selection - 프라핏DSP](../assets/images/addirect/airbridge/ab180_1.png)

4. Once `프라핏DSP` is selected as the channel, the following parameters — **캠페인 (Campaign), 광고 그룹 (Ad Group), 광고 소재 (Ad Creative)** — are automatically populated with preset values as shown in the table below.

> ⚠️ **Caution**: The pre-mapped Preset parameters are configured to collect Addirect data. It is **strongly recommended to use them as-is**. Modifying them may cause data collection issues.

| Item                    | Parameter      | Preset Value | Description                                            | Note                            |
| ----------------------- | -------------- | ------------ | ------------------------------------------------------ | ------------------------------- |
| 채널 (Channel)          | `utm_source`   | `프라핏DSP`  | The advertising channel for the tracking link.         | **Required** (Manual selection) |
| 캠페인 (Campaign)       | `utm_campaign` | `{cmp_nm}`   | Automatically mapped to the Addirect campaign name.    | Preset (Auto-set)               |
| 광고 그룹 (Ad Group)    | `ad_group`     | `{ag_nm}`    | Automatically mapped to the Addirect ad group name.    | Preset (Auto-set)               |
| 광고 소재 (Ad Creative) | `ad_creative`  | `{crtv_nm}`  | Automatically mapped to the Addirect ad creative name. | Preset (Auto-set)               |
| 콘텐츠 (Content)        | `content`      | -            | Enter content-related identifiers as needed.           | Optional (Manual input)         |
| 키워드 (Keyword)        | `keyword`      | -            | Enter keywords associated with the ad as needed.       | Optional (Manual input)         |

5. If needed, configure the **리다이렉트 경로 (Redirect Path)** section below to set the final destination when a user clicks the ad. You can choose from App (Deep Link), App Market, or Website, with separate settings available for each OS (Android/iOS) and Desktop.
6. Once all settings are complete, click the **`✔ 링크 생성 (Create Link)`** button at the bottom of the **트래킹 링크 미리보기 (Tracking Link Preview)** box on the right side.

![Redirect Path Settings and Link Creation](../assets/images/addirect/airbridge/ab180_2.png)

<br><br>

## 3. Verifying and Copying the Tracking Link

### 3.1. Link Creation Complete – Overview of Items

When the link is successfully created, you will be redirected to a page displaying the message **`✔ 트래킹 링크 생성을 완료했습니다 (Tracking link creation completed)`**, along with the generated link details and additional information.

![Tracking Link Creation Complete](../assets/images/addirect/airbridge/ab180_3.png)

The following items can be found on this page:

| Item                                                                        | Description                                                                                                                                                                             |
| --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 트래킹 링크 (조회) (Tracking Link – View)                                   | The standard attribution tracking link used for general mobile and web environments.                                                                                                    |
| **CTV 트래킹 링크 (조회) (CTV Tracking Link – View)**                       | **A tracking link optimized for Connected TV (Smart TV, set-top box, etc.) environments. Includes the `abr_crossplatform=1` parameter to support cross-platform performance tracking.** |
| 상세 정보 > 생성 일시 (Details > Created At)                                | The date and time the tracking link was created.                                                                                                                                        |
| 상세 정보 > 트래킹 링크 (조회) (Details > Tracking Link – View)             | The full tracking link URL with all parameters included.                                                                                                                                |
| **상세 정보 > CTV 트래킹 링크 (조회) (Details > CTV Tracking Link – View)** | **The full CTV tracking link URL with all parameters included.**                                                                                                                        |
| 상세 정보 > 생성 타입 (Details > Creation Type)                             | The method used to create the tracking link (e.g., `Dashboard`).                                                                                                                        |

> The top-right corner provides `+ 추가 생성하기 (Create Another)`, `복제 (Duplicate)`, and `관리하기 (Manage)` buttons for creating additional links and managing existing ones.

### 3.2. Selecting and Copying the CTV Tracking Link

For integration with our DSP, you must use the **CTV Tracking Link** instead of the standard tracking link.

> ⚠️ **Important**: You must copy the URL from the **`CTV 트래킹 링크 (조회) (CTV Tracking Link – View)`** item. Please note this is NOT the standard `트래킹 링크 (조회) (Tracking Link – View)`.

1. Locate the **`CTV 트래킹 링크 (조회) (CTV Tracking Link – View)`** item on the link creation completion page or in the detailed information section.
2. Click the **copy icon (📋)** on the left side of the item to copy the full CTV tracking link URL to your clipboard.
3. The copied CTV tracking link URL will look like this:

```
https://abr.ge/@[service]/motivintelligence?ad_creative={ads_id}&campaign={campaign}&click_id={call_id}&ip={ip}&routing_short_id=3vtxh4&sub_id={pub_idx}&sub_id_1={tagid}&tracking_template_id=3f2ec6c3fc6b3e7508e8f370411556da&user_agent={ua}&ad_type=view&abr_crossplatform=1
```

> 💡 **Note**: The `@[service]` portion of the URL is automatically replaced with the service (app) name configured in the Airbridge dashboard. Since this value varies by service, please use the generated link as-is.

<br><br>

## 4. Registering on the Addirect App Integration Management

Register the **CTV Tracking Link** copied from the Airbridge dashboard into Addirect to finalize the integration.

![Addirect App Integration Management](../assets/images/addirect/airbridge/addirect_dsp_1.png)

1. Log in to the Addirect platform and navigate to the **`앱 연동 관리 (App Integration Management)`** menu.
2. **앱 연동 선택 (App Integration Selection)**: Select **`AirBridge`** from the dropdown at the top.
3. **앱 연동명 (App Integration Name)**: Enter a name to identify this integration.
4. **iOS URL / Android URL**: Enter the app's store URLs for each platform.
5. **트래커 (Tracker)**: Paste the **CTV Tracking Link** you copied earlier into the **`트래커 (Tracker)`** input field.
6. Click the **`등록 (Register)`** button to complete the integration.

<br><br>

## 5. Configuring Postbacks in Airbridge

Once the Addirect app integration has been registered, configure **postback sending** in Airbridge so that event data can be sent back to Addirect.

### 5.1. Navigating to the Postback Settings Menu

1. In the Airbridge dashboard, open the detail page of the **`프라핏DSP`** ad channel selected earlier when creating the tracking link.
2. Select **`Postback > Configuration`** from the top tab menu.
3. If **`Enhanced Privacy Control (EPC)`** is enabled, turn it **off** before proceeding.
4. Some ad channels require additional credentials before postback setup. If **`Postback Keys`** or a credential management option appears, register the required information first by following the relevant channel guide.

![Airbridge Postback Settings and EPC Toggle](../assets/images/addirect/airbridge/ab180_propfit_postback.png)

> 💡 **Note**: Access to postback settings may vary depending on user permissions. In general, Owner and In-house Marketer users can configure them by default, while Agency users need channel data access permission.

> ⚠️ **Important**: **Enhanced Privacy Control (EPC)** protects data from users on iOS 14.5+ who did not consent to ATT by preventing their data from being shared with external channels or services. If EPC remains enabled, postbacks may not include identifier-level user data, so it is recommended to **disable EPC** for Addirect integration.

### 5.2. Enabling Postback Sending and Selecting Events

1. Turn on the **`Send postbacks`** toggle, then click **`Postback event setup`**.
2. In the **event selection** step, choose the events you want to send to Addirect.
3. Only events that have actually been collected by Airbridge will appear in the list. If a required event is missing, first confirm that the event is being tracked in the app.
4. Select at least one event, then click **`Next`**.

![Airbridge Postback Event Selection](../assets/images/addirect/airbridge/ab180_propfit_postback_2.png)

> ⚠️ **Caution**: If no events are selected, actual postback sending will not start even when the `Send postbacks` toggle is enabled.

### 5.3. Configuring Event Data and Delivery Rules

Configure the **event data** and **delivery rules** for each selected event.

1. In **`Edit event details`**, review the event name that will be sent to the ad channel.
2. If supported by the channel, decide whether to include **revenue data** for each event.
3. In **`Edit delivery rule`**, select the options that match your campaign objective.
4. Click **`Save`** after completing the configuration.

| Item                | Recommended setting | Description                                                                                                        |
| ------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Attribution         | `Attributed events` | Sends only events attributed to the channel. Suitable for standard performance measurement or settlement purposes. |
| Attribution         | `All events`        | Sends all events regardless of attribution. Useful when retargeting optimization requires broader event sharing.   |
| First or All Events | `First events only` | Sends only the first occurrence per device. Suitable for CPI/CPA-oriented operation.                               |
| First or All Events | `All events`        | Sends repeated events as well. Useful for re-engagement or retargeting optimization.                               |

> 💡 **Operational tip**: For retargeting-focused campaigns, Airbridge recommends considering **`Attribution = All events`** and **`First or All Events = All events`**.

> ⚠️ **Caution**: Editing the postback URL can directly affect campaign optimization and billing. If URL changes are required, it is recommended to coordinate with the Addirect team in advance.

### 5.4. Applying and Verifying the Postback Settings

1. After saving all event settings, click **`Apply`**.
2. Once applied, Airbridge begins sending the selected event postbacks to Addirect **immediately**.
3. If you update a postback configuration that is already active, the change is reflected as soon as it is applied.
4. After the initial setup, verify with the Addirect side that events are being received as expected.

<br><br>

## 6. References

- [Airbridge - Creating Tracking Links on the Dashboard](https://help.airbridge.io/en/guides/creating-tracking-links-on-the-dashboard)
- [Airbridge - Postback Settings](https://help.airbridge.io/en/guides/postback-settings-new)
- [Addirect - App Integration Management Guide](https://addi-1.gitbook.io/guide/part-7./undefined-1)
