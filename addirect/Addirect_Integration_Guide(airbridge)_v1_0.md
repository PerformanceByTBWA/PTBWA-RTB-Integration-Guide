# ADDirect Integration Guide for Airbridge v1.0

---

- [1. Overview](#1-overview)
- [2. Creating a Tracking Link](#2-creating-a-tracking-link)
  - [2.1. Navigating to the Tracking Link Menu](#21-navigating-to-the-tracking-link-menu)
  - [2.2. Selecting the Channel and Configuring Optimization Parameters](#22-selecting-the-channel-and-configuring-optimization-parameters)
- [3. Verifying and Copying the Tracking Link](#3-verifying-and-copying-the-tracking-link)
  - [3.1. Link Creation Complete – Overview of Items](#31-link-creation-complete--overview-of-items)
  - [3.2. Selecting and Copying the CTV Tracking Link](#32-selecting-and-copying-the-ctv-tracking-link)
- [4. Registering on the ADDirect App Integration Management](#4-registering-on-the-addirect-app-integration-management)

---

<br><br>

## 1. Overview

> This document explains how advertisers can create a tracking link for ADDirect on the Airbridge dashboard and register it in the ADDirect service to complete the integration.

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

> ⚠️ **Caution**: The pre-mapped Preset parameters are configured to collect ADDirect data. It is **strongly recommended to use them as-is**. Modifying them may cause data collection issues.

| Item                    | Parameter      | Preset Value | Description                                            | Note                            |
| ----------------------- | -------------- | ------------ | ------------------------------------------------------ | ------------------------------- |
| 채널 (Channel)          | `utm_source`   | `프라핏DSP`  | The advertising channel for the tracking link.         | **Required** (Manual selection) |
| 캠페인 (Campaign)       | `utm_campaign` | `{cmp_nm}`   | Automatically mapped to the ADDirect campaign name.    | Preset (Auto-set)               |
| 광고 그룹 (Ad Group)    | `ad_group`     | `{ag_nm}`    | Automatically mapped to the ADDirect ad group name.    | Preset (Auto-set)               |
| 광고 소재 (Ad Creative) | `ad_creative`  | `{crtv_nm}`  | Automatically mapped to the ADDirect ad creative name. | Preset (Auto-set)               |
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

## 4. Registering on the ADDirect App Integration Management

Register the **CTV Tracking Link** copied from the Airbridge dashboard into ADDirect to finalize the integration.

![ADDirect App Integration Management](../assets/images/addirect/airbridge/addirect_dsp_1.png)

1. Log in to the ADDirect platform and navigate to the **`앱 연동 관리 (App Integration Management)`** menu.
2. **앱 연동 선택 (App Integration Selection)**: Select **`AirBridge`** from the dropdown at the top.
3. **앱 연동명 (App Integration Name)**: Enter a name to identify this integration.
4. **iOS URL / Android URL**: Enter the app's store URLs for each platform.
5. **트래커 (Tracker)**: Paste the **CTV Tracking Link** you copied earlier into the **`트래커 (Tracker)`** input field.
6. Click the **`등록 (Register)`** button to complete the integration.

<br><br>

## 5. References

- [Airbridge - Creating Tracking Links on the Dashboard](https://help.airbridge.io/ko/guides/creating-tracking-links-on-the-dashboard)
- [ADDirect - App Integration Management Guide](https://addi-1.gitbook.io/guide/part-7./undefined-1)
