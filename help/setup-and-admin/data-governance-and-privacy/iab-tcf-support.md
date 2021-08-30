---
title: IAB TCF 2.0支援Audience Manager
description: Adobe可讓您透過選擇加入功能和IAB透明與同意架構2.0(TCF 2.0)支援的Audience Manager外掛程式，管理使用者的隱私權選擇，並與使用者針對該選擇溝通。 本文與本檔案一起運作，可協助您了解IAB TCF的Audience Manager外掛程式，以及外掛程式如何與Adobe的選擇加入物件和同意管理提供者(CMP)搭配運作。
feature: Data Governance & Privacy
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '1120'
ht-degree: 0%

---

# IAB TCF 2.0支援Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe可讓您透過選擇加入功能和IAB透明與同意架構2.0(TCF 2.0)支援的Audience Manager外掛程式，管理使用者的隱私權選擇，並與使用者針對該選擇溝通。 本文與本檔案一起運作，可協助您了解IAB TCF的Audience Manager外掛程式，以及外掛程式如何與Adobe的選擇加入物件和同意管理提供者(CMP)搭配運作。 若要進一步了解IAB，請參閱其網站[https://www.iabeurope.eu/](https://www.iabeurope.eu/)。

## 第一步：了解ECID的選擇加入 {#first-step-understand-ecid-s-opt-in}

若要了解如何使用IAB TCF，您必須先了解[!DNL Opt-in]功能，這是Experience CloudID服務(ECID)程式庫的一部分。 如果您不熟悉選擇加入的運作方式，請先參閱[這篇實用的文章](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)。 您也應該檢閱選擇加入[檔案](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html)。 完成這些資源後，請返回本頁面並繼續。

## 適用於IAB TCF的Audience Manager外掛程式 {#the-audience-manager-plug-in-for-iab-tcf}

現在您至少已基本了解選擇加入服務的運作方式，Audience Manager可將其[!DNL IAB Transparency and Consent Framework (TCF)]支援層級化，這可透過外掛程式加入選擇加入物件。

適用於IAB TCF的Audience Manager外掛程式擴充了選擇加入的功能，讓AAM客戶能根據IAB TCF評估、遵守使用者隱私權選擇，並轉送給下游合作夥伴。 它提供資料控管單位(即您是Adobe客戶)和廠商(DMP、DSP、SSP、廣告伺服器等)的標準 可用來跨同意範圍了解同意。

## 啟用IAB TCF {#enabling-iab-tcf}

如果您使用Adobe Experience Platform Launch，啟用IAB TCF適用的Audience Manager外掛程式就很輕鬆，因為這是簡單的核取方塊，如下方短片所示：

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

或者，如果您未使用Launch，則可在實例化Experience Cloud訪客時使用`isIabContext=true`來啟用它。 這會起始IAB TCF流程，亦即新增另一個步驟以收集同意，使用IAB TCF來查詢IAB TC字串，並將其提供回「選擇加入」，而「選擇加入」接著與Experience Cloud解決方案通訊。

## IAB TC字串 {#iab-tcf-consent-string}

IAB提供的標準之一是「同意字串」（也稱為「DaisyBit」），這實際上是兩份清單加總：

1. 用途：**什麼**&#x200B;同意執行？
1. 供應商：**誰**&#x200B;同意？

### 用途 {#purposes}

若使用IAB TCF 2.0，收集同意有十種「用途」（廠商可以對訪客的資料做什麼）。 Adobe Audience Manager不要求全部10個，但除了廠商同意外，僅要求以下目的的同意：

* **用途1:** 在裝置上儲存和/或存取資訊；
* **目的10:** 開發和改進產品；
* **特殊用途1:** 確保安全性、防止欺詐及除錯。

這是IAB TC字串的第一部分，只記錄為1和0，指定該用途/活動是否已核准。

>[!NOTE]
>
>根據IAB法規，特殊用途1（確保安全性、防止詐騙和除錯）一律同意，且使用者無法反對。

### 供應商 {#vendors}

IAB TC字串的另一部分是數百家廠商的長清單，讓訪客可以看到網站上有標籤的適用廠商清單，並可以選擇要使用哪些廠商。 供應商在清單中保持其地位。 例如，此清單上的Adobe Audience Manager廠商編號為565。 如果清單中的號碼有「1」，則Audience Manager可從清單前面執行已核准的用途。 如果AAM競價為「0」，則無法對資料執行任何動作。

**若要Audience Manager提供UI，供客戶使用IAB TCF來選擇這些用途和廠商，或核准/不批准所有活動，您必須使用已向IAB TCF註冊的CMP合作夥伴，或建立支援IAB TCF且已向IAB TCF註冊的CMP合作夥伴。**

## 選擇加入：在IAB和Adobe解決方案之間轉譯 {#opt-in-translating-between-iab-and-adobe-solutions}

使用IAB TCF的其中一項好處是，上述標準用途可能比Adobe解決方案清單讓使用者更清楚了解他們要核准的項目。 最終用戶可能不知道「批准」Audience Manager或[!DNL Target]意味著什麼，但「在設備上儲存和/或訪問資訊」或「開發和改進產品」可能更容易理解並同意。

為了讓Audience Manager得到核准(亦即為了轉譯選擇加入的IAB用途以給予AAM「是」投票)，目的1和10（如上所列）必須獲得使用者的同意。 如果其中一個未獲核准，或供應商未獲核准，AAM將不會執行像素引發或設定Cookie。 同樣很好的是，許多客戶只是選擇提供「全部或全部」的UI，這當然會允許或不允許使用Audience Manager(以及其他Experience Cloud解決方案)。

[說明檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en)中提供一些關於適用於IAB TCF的Audience Manager外掛程式如何同時套用至發佈商和廣告商使用案例的絕佳資訊。

## IAB:傳送同意下游 {#iab-sending-consent-downstream}

使用IAB TCF適用的Audience Manager外掛程式時，使用者的同意選擇也會傳送至全球廠商清單上合作夥伴的平台層級（第三方）ID同步，讓合作夥伴擁有使用者同意資訊，並可依此資訊採取動作。 此資訊會以兩個變數傳送：

* gdpr = 1
* gdpr_consent = [編碼同意字串&lt;a1/]

請注意，如果使用者處於IAB內容中，且未提供同意（或提供負面同意），則Audience Manager完全不會收集IAB TC字串，因此會捨棄呼叫。 所以，在這種情況下，不要通過下游的同意。

## 示範 {#demo}

在以下影片中，查看ECID和解決方案的Cookie和信標如何受到IAB使用者選擇的影響。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

如需適用於IAB TCF 2.0的Audience Manager外掛程式的詳細資訊，包括如何實作和測試、使用案例和工作流程，請參閱[檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)。
