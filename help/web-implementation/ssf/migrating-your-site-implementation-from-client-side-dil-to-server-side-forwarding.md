---
title: 將您網站的AAM實作從用戶端DIL移轉至伺服器端轉送
description: 如果您同時擁有Adobe Audience Manager(AAM)和Adobe Analytics，而且您目前使用「DIL」（資料整合庫）程式碼從頁面傳送點擊至AAM，並且從頁面傳送點擊至Adobe Analytics，本教學課程將適用於您。 由於您同時擁有這兩種解決方案，而且這兩種解決方案都屬於Adobe Experience Cloud，因此您有機會遵循開啟「伺服器端轉送(SSF)」的最佳實務，這可讓Analytics資料收集伺服器即時將網站分析資料轉送至Audience Manager，而不需讓用戶端程式碼從頁面傳送額外點擊至AAM。 本教學課程將引導您完成從舊版「用戶端DIL」實作切換至較新「伺服器端轉送」方法的步驟。
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
translation-type: tm+mt
source-git-commit: 133279f589bd58aef36a980c2b7248ae00fd9496
workflow-type: tm+mt
source-wordcount: '2319'
ht-degree: 0%

---


# 將您網站的AAM實作從 [!DNL Client-Side] DIL移轉至 [!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

如果您同時擁有Adobe Audience Manager(AAM)和Adobe Analytics，而且您目前使用「DIL」([!DNL Data Integration Library])程式碼從頁面傳送點擊至AAM，並且從頁面傳送點擊至Adobe Analytics，則本教學課程適用於您。 由於您同時擁有這兩種解決方案，而且這兩種解決方案都屬於Adobe Experience Cloud，因此您有機會遵循開啟「[!DNL Server-Side Forwarding] (SSF)」的最佳實務，這可讓資料收集伺服器即時將網站分析資料轉送至Audience Manager，而不需讓 [!DNL Analytics][!DNL client-side] code從頁面傳送額外點擊至AAM。 本教學課程將引導您完成從舊版「[!DNL Client-Side DIL]」實作切換至新版「[!DNL Server-Side forwarding]」方法的步驟。

## [!DNL Client-Side] (DIL)與 [!DNL Server-Side] {#client-side-dil-vs-server-side}

比較這兩種將Adobe Analytics資料匯入AAM的方法並加以比較時，首先可能會有助於在下列影像中呈現差異：

![從用戶端到伺服器端](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DIL實作 {#client-side-dil-implementation}

如果您使用此方法將Adobe Analytics資料匯入AAM，表示您的網頁有兩個點擊：一個會 [!DNL Analytics]前往AAM，一個會前往AAM(在複製網頁 [!DNL Analytics] 上的資料後)。 [!UICONTROL Segments] 會從AAM傳回至頁面，供其用於個人化等。 這被視為「舊版」實作，不再建議使用。

除了未遵循最佳實務外，使用此方法的缺點包括：

* 來自頁面的兩次點擊，而非僅一次
* [!UICONTROL Server-Side Forwarding] 是AAM觀眾即時分享的必要 [!DNL Analytics]條件， [!DNL Client-side] 因此實作不允許此功能（未來可能還有其他功能）

建議您改用AAM實 [!UICONTROL Server-Side Forwarding] 作方法。

### [!UICONTROL Server-Side Forwarding] 實施 {#server-side-forwarding-implementation}

如上圖所示，點擊來自網頁至Adobe Analytics。 [!DNL Analytics] 然後即時將該資料轉送至AAM，並評估訪客至AAM [!UICONTROL traits] 和 [!UICONTROL segments]，就像點擊是直接從頁面產生。

[!UICONTROL Segments] 會在相同的即時點擊中傳回， [!DNL Analytics]將回應轉送至網頁以進行個人化等。

移至伺服器端轉送不會造成時間上的不便。 我們強烈建議任何同時擁有Audience Manager並運用此實 [!DNL Analytics] 施方法的人。

## 您有兩個主要任務 {#you-have-two-main-tasks}

這個網頁上有很多資訊，當然，這都很重要。 不過，這 **一切歸結為兩個您需要做的主要工作**:

1. 將您的程式碼從 [!DNL Client-Side] DIL程式碼變更為程 [!UICONTROL Server-Side Forwarding] 式碼
1. 在中翻動交 [!DNL Analytics] 換機 [!DNL Admin Console] 以開始實際轉發資料(每 [!UICONTROL report suite]個)

如果跳過其中任一步驟，SSF將無法正常工作。 步驟和其他資料已新增至本檔案，以協助您正確執行這兩個步驟以進行設定。

## 實施選項 {#implementation-options}

當您從程式碼 [!DNL client-side] 移至 [!DNL server-side]程式碼時，您要執行的其中一項工作是將程式碼變更為新程 [!UICONTROL Server-Side Forwarding] 式碼。 這是使用下列其中一個選項來完成：

* Adobe Experience Platform Launch —— 我們針對網頁屬性建議的實作選項。 您將會看到，這是一項非常簡單的工作， [!DNL Launch] 因為您已經完成了所有艱難的工作。
* 在頁面上——如果您尚未（目前）使用Adobe Launch, `doPlugins` 您也可以將新的SSF程 [!DNL appMeasurement.js] 式碼直接放入檔案的函式中
* 其他標籤管理器——這些標籤可以視為與上一個（在頁面上）選項一樣，因為您仍會將SSF代碼放入 `doPlugins`，而其他標籤管理器正在儲存代碼的任何位 [!DNL AppMeasurement] 置

我們將在「更新程式碼」區段中檢視以下各項。

## 實施步驟 {#implementation-steps}

### 步驟0:先決條件：Experience Cloud ID服務(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

移轉至的主要先決條件 [!UICONTROL Server-Side Forwarding] 是必須實作Experience Cloud ID服務。 如果您使用Experience Platform Launch，這最容易做到，在此情況下，您只需安裝ECID擴充功能，就可完成其他工作。

如果您使用非Adobe TMS或完全沒有TMS，請實作ECID以在任何其他Adobe解決方案 **之前** 執行。 如需詳細 [資訊，請參閱](https://marketing.adobe.com/resources/help/en_US/mcvid/) ECID檔案。 唯一的先決條件是有關程式碼版本，因此，只要在下列步驟中套用最新版本的程式碼，您就沒問題。

>[!NOTE]
>
>在實施之前，請閱讀本整份檔案。 下方的「時間」區段包含您應於何 *時實作* 每件作品的重要資訊，包括ECID（如果尚未實作）。

### 步驟1:從DIL程式碼記錄目前使用的選項 {#step-record-currently-used-options-from-dil-code}

當您準備從 [!DNL Client-Side] DIL程式碼移至 [!UICONTROL Server-Side Forwarding]AAM時，第一步就是識別您使用DIL程式碼所做的一切，包括自訂設定和傳送至AAM的資料。 需要注意和考慮的事項包括：

* 一般 [!DNL Analytics] 變數，使用 [!DNL siteCatalyst.init][!DNL Analytics] DIL模組——您不必擔心這個變數，因為它的工作只是傳送一般變數過去，而且只要啟用SSF即可。
* 合作夥伴子網域——在DIL.create函式中，記下參數 `partner` 。 這稱為「合作夥伴子網域」，有時稱為「合作夥伴ID」，當您放置新的SSF代碼時，將會需要它。
* [!DNL Visitor Service Namespace] -也稱為「[!DNL Org ID]」或「[!DNL IMS Org ID]」，當您設定新的SSF代碼時，也需要這個選項。 記下來。
* containerNSID、uuidCookie和其他進階選項——記下您使用的任何其他進階選項，以便您也能在SSF程式碼中設定這些選項。
* 其他頁面變數——如果其他變數是從頁面傳送至AAM（除了由siteCatalyst.init處理的一般變數外），您還需要記下這些變數，以便透過SSF傳送(劇透警報： [!DNL Analytics] 變數) [!DNL contextData] 中。

### 步驟2:更新代碼 {#step-updating-the-code}

在上述「實作選項」一節中，會針對您實作的方式／地點提供多個選項 [!UICONTROL Server-Side Forwarding]。 為了讓本節有效，我們必須將其分為以下幾節（其中兩節合併）。 請至本節最能說明您需求的方法。

#### Adobe Experience Platform Launch {#launch-by-adobe}

觀看以下影片，瞭解如何將實作選項從 [!DNL Client-Side] DIL程式碼移至Experience Platform Launch [!UICONTROL Server-Side Forwarding] 中。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### 「頁面上」或非Adobe標籤管理器 {#on-the-page-or-non-adobe-tag-manager}

觀看以下影片，瞭解如何將實作選項從 [!DNL Client-Side] DIL程式碼移入程式碼中， [!UICONTROL Server-Side Forwarding][!DNL AppMeasurement] 置於檔案中或非Adobe標籤管理系統中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 步驟3:啟用轉發(每 [!UICONTROL Report Suite]個) {#step-enabling-the-forwarding-per-report-suite}

在本教學課程中，我們已將所有時間花在將程式碼從程式碼切換 [!DNL Client-Side DIL] 為程式碼 [!UICONTROL Server-Side Forwarding]上。 這沒關係，因為那是比較難的部分。 雖然您會看到本節非常簡單，但與更新程式碼一樣重要。 在此影片中，您將瞭解如何切換開關，以啟用將資料從Analytics轉送至Audience Manager的實際功能。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 如影片中所述，請記住，在Experience Cloud後端完全啟用轉送功能需要4小時。

## 計時 {#timing}

提醒您，從移動到移動有兩個主要 [!DNL Client-Side DIL] 任務 [!UICONTROL Server-Side Forwarding]:

1. 更新程式碼
1. 在 [!DNL Analytics] [!DNL Admin Console]

但問題是，你先做哪一個？ 重要嗎？ 好，抱歉，有兩個問題。 但答案是……取決於情況，是的，這可 *能* 。 這樣很模糊，又如何？ 讓我們來劃分。 但首先，如果您是擁有大量網站的大型組織，還會出現另一個問題：我必須一次做所有事嗎？ 那個比較容易。 不。 你可以一件件地做，差不多。:)

### 深入探討 {#a-little-deeper-dive}

時間和順序問題的原因在於轉發*實際*如何運作，這可概括為以下幾個技術事實：

* 如果您已實作Experience Cloud ID服務(ECID)，而且開啟 [!DNL Analytics] （「開關」）中的開關，資料會從 [!DNL Admin Console][!DNL Analytics] AAM轉送，即使您尚未更新程式碼亦然。
* 如果沒有實作ECID，即使您已開啟切換器，而且有SSF代碼，資料也不會轉送。
* SSF代碼(無論在頁 [!DNL Launch] 面中還是頁面上)確實處理響應，當然，完成遷移是必需的。
* 請記住，SSF開關是由啟 [!UICONTROL Report Suite]用的，但代碼是由中的屬性處理的， [!DNL Launch]或 [!DNL AppMeasurement] 者是由檔案(如果您不使用 [!DNL Launch]

### 最佳實務 {#best-practices}

根據這些技術細節，以下是有關「何時該做什麼」時機的建議：

#### 如果您尚未實作ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 針對要為SSF啟 [!DNL Analytics] 用的每 [!UICONTROL report suite] 個交換機，將交換機反向

   1. 轉送尚未開始，因為您沒有ECID

1. 依據網站，將程式碼從更 [!DNL Client-Side DIL] 新為SSF(此程式碼可能位 [!DNL Launch] 於頁面中，或位於頁面上，如上文另一節所述)

   1. 轉送功能現在會流動（如您已新增ECID），您也應該收到對信標的正確JSON回應（如需詳細資訊，請參閱下方的「驗證與疑難排解」一節） [!DNL Analytics]

#### 如果您已實作ECID {#if-you-do-have-ecid-implemented}

1. 準備並規劃，以便您準備好將程式碼從DIL更新為SSF PER, [!UICONTROL report suite] 以便為SSF啟用：

   1. 在中翻轉交換機 [!DNL Analytics] 以啟用SSF

      1. 轉發將啟動，因為您已啟用ECID
   1. 請盡快將您的程式碼從更新 [!DNL Client-Side DIL] 至SSF(此程式碼可能在頁 [!DNL Launch] 面中或頁面上，如上節所述)

      1. 您應該會收到對信標的正確JSON回 [!DNL Analytics] 應（如需詳細資訊，請參閱下方的驗證與疑難排解一節）


**附註1:** 請務必盡可能接近這兩個步驟，因為在上述步驟1和2之間，您將會有將資料匯入AAM的複製。 換言之，SSF將開始從傳送資料至 [!DNL Analytics] AAM，而且由於DIL程式碼仍在頁面上，因此也會有點擊從頁面直接傳入AAM，因此資料倍增。 當您將程式碼從DIL更新至SSF時，就會有所緩解。

**附註2:** 如果您更希望在資料上有小的差異，而不是在資料上有小的重複，則可以切換上述步驟1和2的順序。 將程式碼從DIL移至SSF將會停止資料流入AAM，直到您能夠翻轉開關以開啟SSF [!UICONTROL report suite]。 通常客戶寧願將資料加倍一小部分，也不願錯過將訪客帶入和 [!UICONTROL traits] 的機 [!UICONTROL segments]會。

#### 在您擁有許多網站和 [!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

本主題在前幾節中簡要介紹，主要策略可概括如下：

一次移轉一個網站/[!UICONTROL report suite] (或一組網站/[!UICONTROL report suites])。

不過，這可能會因為幾個可能的情況而變得有些棘手：

* 您的網站包含數個 [!UICONTROL report suites]
* 您有包含 [!UICONTROL report suite] 數個網站(例如全域網站 [!UICONTROL report suite])
* 您使用一個屬 [!DNL Launch] 性來覆蓋數個網站
* 您有不同的開發團隊，負責不同的網站

因為有了這些項目，它會變得有些複雜。 我能建議的最好事情是：

* 請花點時間，根據上述說明制定移轉至SSF的策略
* 根據（或單一檔案）中的單一屬性通常對應至1或2個不同 [!DNL Launch][!DNL AppMeasurement][!UICONTROL report suites]，您可能可以逐一針對這些不同群組制定計畫，將您的企業更新為SSF
* 如果您正在與Adobe Consulting合作，請與他們討論有關您的移轉計畫，以便他們能夠視需要提供協助

## 驗證和疑難排解 {#validation-and-troubleshooting}

驗證是否已啟動並執 [!UICONTROL Server-Side Forwarding] 行的主要方式，是查看您對應用程式中任何Adobe Analytics點擊的回應。

如果您未從Audience Manager [!UICONTROL server-side forwarding] 處理資料， [!DNL Analytics] 則實際上對信標沒有回應( [!DNL Analytics] 除2x2像素外)。 不過，如果您正在執行SSF，則有些項目可在請求和回應中驗證，讓您知道 [!DNL Analytics][!DNL Analytics] Audience Manager的通訊正確、轉送點擊和取得回應。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**警告：** 小心錯誤的「成功」—如果有回應，而且一切似乎都在運作，請確定回應中有「物件」物件。 如果不是，您可能會看到一則訊息 [!DNL "status":"SUCCESS"]。 雖然這聽起來很瘋狂，但這實際上證明它無法正常運作。 如果您看到這一點，表示您已在或中完成代碼更 [!DNL Launch] 新 [!DNL AppMeasurement]，但中的轉發 [!DNL Analytics] 尚未 [!DNL Admin Console] 完成。 在這種情況下，您需要驗證是否已在中為您的SSF [!DNL Analytics] 啟用 [!DNL Admin Console] 了 [!UICONTROL report suite]。 如果您已經有，而且還沒有4小時，請耐心等待，因為在後端進行所有必要的變更可能需要這麼長時間。

![假成功](assets/falsesuccess.png)

如需詳細資 [!UICONTROL Server-Side Forwarding]訊，請參閱 [檔案](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)。
