---
title: 將網站的Audience Manager實作從使用者端DIL移轉至伺服器端轉送
description: 瞭解如何將網站的Audience Manager(AAM)實作從使用者端DIL移轉至伺服器端轉送。 如果您同時擁有AAM和Adobe Analytics，且使用DIL(Data Integration Library)代碼將點選從頁面傳送至AAM，也已將點選從頁面傳送至Adobe Analytics，則本教學課程適用。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# 將網站的Audience Manager實作從使用者端DIL移轉至伺服器端轉送 {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

如果您同時擁有Adobe Audience Manager (AAM)和Adobe Analytics AAM，而且您目前正在使用DIL([!DNL Data Integration Library])程式碼，以及從頁面傳送點選至Adobe Analytics。 由於您同時擁有這兩個解決方案，而且它們都是Adobe Experience Cloud的一部分，因此您有機會遵循開啟伺服器端轉送的最佳做法，此做法可啟用 [!DNL Analytics] 資料收集伺服器可將網站分析資料即時轉送至Audience Manager，而非讓使用者端程式碼從頁面傳送其他點選至AAM。 本教學課程將逐步帶您瞭解從舊版使用者端DIL實作切換至新版伺服器端轉送方法的步驟。

## 使用者端(DIL)與伺服器端 {#client-side-dil-vs-server-side}

比較和比較這兩種將Adobe Analytics資料匯入AAM的方法時，先將下列影像的差異視覺化可能有所幫助：

![使用者端對伺服器端](assets/client-side_vs_server-side_aam_implementation.png)

### 使用者端DIL實作 {#client-side-dil-implementation}

如果您使用此方法將Adobe Analytics資料匯入AAM，會有兩個點選來自您的網頁：一個將前往 [!DNL Analytics]，且其中一個會前往AAM (在複製 [!DNL Analytics] 網頁上的資料。 [!UICONTROL Segments] 會從AAM傳回至頁面，並用於個人化等。 這將視為舊版實作，不再建議使用。

除了這並非遵循最佳實務之外，使用此方法的缺點包括：

* 來自頁面的兩個點選，而非只有一個
* 需要伺服器端轉送才能即時將AAM對象分享到 [!DNL Analytics]，因此使用者端實施不允許此功能（以及未來可能的其他功能）

建議您改用AAM實作的伺服器端轉送方法。

### 伺服器端轉送實作 {#server-side-forwarding-implementation}

如上圖所示，點選來自指向Adobe Analytics的網頁。 [!DNL Analytics] 然後將該資料即時轉送至AAM，並評估訪客的AAM特徵和 [!UICONTROL segments]，就像點選直接來自頁面一樣。

[!UICONTROL Segments] 在相同即時點選上傳回至 [!DNL Analytics]，會將回應轉送至網頁以進行個人化等。

改用伺服器端轉送並無時間上的考量。 Adobe強烈建議同時擁有Audience Manager和 [!DNL Analytics] 使用此實作方法。

## 您有兩個主要任務 {#you-have-two-main-tasks}

本頁有不少資訊，當然也很重要。 不過， **所有要點歸結為您需要做的兩項主要工作**：

1. 將您的程式碼從使用者端DIL程式碼變更為伺服器端轉送程式碼
1. 將切換器翻轉 [!DNL Analytics] [!DNL Admin Console] 開始實際轉送資料(根據 [!UICONTROL report suite])

如果您略過其中一項工作，伺服器端轉送將無法正常運作。 本檔案新增了步驟和其他資料，可幫助您正確執行這兩個步驟以進行設定。

## 實作選項 {#implementation-options}

當您從使用者端轉送移至伺服器端轉送時，其中一個工作是將程式碼變更為新的伺服器端轉送程式碼。 使用下列其中一個選項完成此操作：

* Adobe Experience Platform標籤 — Adobe建議的Web屬性實作選項。 您會發現這是一項輕鬆的工作，因為Platform標籤已為您完成所有艱難的工作。
* 在頁面上 — 您也可以將新的SSF程式碼直接放入 `doPlugins` 函式於 `appMeasurement.js` 檔案(如果尚未使用Adobe啟動)
* 其他標籤管理員 — 這些標籤管理員的處理方式與上一個（在頁面上）選項相同，因為您仍會將SSF程式碼放入 `doPlugins`，其他標籤管理員要儲存的位置 [!DNL AppMeasurement] 程式碼

我們將在以下各節中檢視 _更新程式碼_ 區段。

## 實作步驟 {#implementation-steps}

下列步驟說明實作。

### 步驟0：先決條件：Experience CloudID服務(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

移至伺服器端轉送的主要先決條件為實作Experience CloudID服務。 如果您使用Experience Platform Launch，最輕鬆的方式是完成此步驟，在此情況下，您只需安裝ECID擴充功能，其他功能便可順利完成。

如果您使用非AdobeTMS或完全沒有TMS，請實作ECID以執行 **早於** 任何其他Adobe解決方案。 請參閱 [ECID檔案](https://experienceleague.adobe.com/docs/id-service/using/home.html) 以取得更多詳細資料。 唯一的其他先決條件與程式碼版本有關，因此只要在下列步驟中套用程式碼的最新版本，就不會有問題。

>[!NOTE]
>
>實作前請先詳閱整份檔案。 以下的「計時」部分包含有關以下專案的重要資訊 *時間* 您應該實作每個片段，包括ECID （如果尚未實作）。

### 步驟1：從DIL程式碼記錄目前使用的選項 {#step-record-currently-used-options-from-dil-code}

當您準備好從使用者端DIL程式碼移至伺服器端轉送時，第一步是識別您使用DIL程式碼執行的所有操作，包括自訂設定和傳送至AAM的資料。 注意事項和考量事項包括：

* 一般 [!DNL Analytics] 變數，使用 `siteCatalyst.init` DIL模組 — 您無需擔心此模組，因為其工作僅是傳送正常訊息 [!DNL Analytics] 變數超過，且只需啟用伺服器端轉送即可完成。
* 合作夥伴子網域 — 在 `DIL.create` 函式，記下 `partner` 引數。 這稱為您的「合作夥伴子網域」，有時也稱為「合作夥伴ID」，當您放置新的伺服器端轉送程式碼時，系統會用到它。
* [!DNL Visitor Service Namespace]  — 也稱為您的&quot;[!DNL Org ID]「或」[!DNL IMS Org ID]，」當您設定新的伺服器端轉送程式碼時，也需要此專案。 記下它。
* containerNSID、uuidCookie和其他進階選項 — 記下您正在使用的任何其他進階選項，以便在伺服器端轉送程式碼中設定它們。
* 其他頁面變數 — 如果從頁面將其他變數傳送到AAM （除了正常變數以外） [!DNL Analytics] 由siteCatalyst.init處理的變數)，您需要記下這些變數，以便它們可以透過伺服器端轉送來傳送(破壞程式警報：透過 [!DNL contextData] 變數)。

### 步驟2：更新程式碼 {#step-updating-the-code}

在 [實作選項](#implementation-options) （上文）針對您實作伺服器端轉送的方式和位置提供了多個選項。 為了讓此區段生效，我們需要將其分成這些區段（其中兩個區段合併）。 請移至本節最能描述您需求的方法。

#### Adobe Experience Platform標籤 {#launch-by-adobe}

觀看以下影片，瞭解如何在Experience Platform Launch中將實作選項從使用者端DIL程式碼移動到伺服器端轉送。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### 「在頁面上」或非Adobe標籤管理員 {#on-the-page-or-non-adobe-tag-manager}

觀看以下影片，瞭解如何在中將實作選項從使用者端DIL程式碼移動到伺服器端轉送 [!DNL AppMeasurement] 代碼，位於檔案或非Adobe標籤管理系統中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 步驟3：啟用轉送(根據 [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

在本教學課程中，到目前為止，我們一直致力於將程式碼從使用者端DIL程式碼切換至伺服器端轉送。 沒關係，因為這是比較困難的部分。 雖然您會看到此區段非常簡單，但與更新程式碼一樣重要。 在本影片中，您將會瞭解如何切換開關，以實際將資料從Analytics轉送至Audience Manager。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 如影片所述，請記住，在Experience Cloud後端完全實作轉送最多需要4小時。

## 計時 {#timing}

提醒您，從使用者端DIL移至伺服器端轉送有兩個主要工作：

1. 更新程式碼
1. 將切換器翻轉到 [!DNL Analytics] [!DNL Admin Console]

但問題是，您首先要做什麼？ 這重要嗎？ 好的，抱歉，這是兩個問題。 但答案是……這要看情況，是的 *可以* 重要。 這表示不明確嗎？ 讓我們來加以劃分。 但首先，如果您是擁有許多網站的大型組織，可能會出現另一個問題：我是否必須一次完成所有工作？ 那個比較簡單。 不適用。 您可以逐一執行。

### 再深入一點 {#a-little-deeper-dive}

時間與順序之所以重要，是因為轉送方式 _確定_ 作品，可歸納為以下幾個技術事實：

* 如果您已實作Experience CloudID服務(ECID)，而且將 [!DNL Analytics] [!DNL Admin Console] （「交換器」）已開啟，資料將會從 [!DNL Analytics] 至AAM，即使您尚未更新程式碼亦然。
* 如果您尚未實作ECID，則即使您已開啟開關，且擁有伺服器端轉送程式碼，資料也不會轉送。
* 伺服器端轉送程式碼（不論在Platform標籤中或頁面上）會真正處理回應，且是完成移轉的必要專案。
* 請記住，伺服器端轉送交換器是由 [!UICONTROL report suite]，但程式碼是由Platform標籤中的屬性處理，或由 [!DNL AppMeasurement] 檔案（如果您未使用Platform標籤）。

### 最佳實務 {#best-practices}

根據這些技術詳細資訊，以下是有關該做什麼和何時做什麼的建議：

#### 如果您尚未實作ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 將切換器翻入 [!DNL Analytics] 針對每個 [!UICONTROL report suite] 您將啟用伺服器端轉送的許可權。

   1. 轉送尚未開始，因為您沒有ECID。

1. 根據網站，將您的程式碼從使用者端DIL更新至伺服器端轉送（這可能在Platform標籤中）或頁面上，如上方另一節所述。

   1. 轉送現在流程（因為您已新增ECID），且您也應收到適當的JSON回應至 [!DNL Analytics] 信標（如需詳細資訊，請參閱下方的「驗證和疑難排解」一節）。

#### 如果您已實作ECID {#if-you-do-have-ecid-implemented}

1. 準備並規劃，以便您準備好將程式碼從DIL更新至伺服器端轉送。 [!UICONTROL report suite] 您將為伺服器端轉送啟用的專案：

   1. 將切換器翻入 [!DNL Analytics] 以啟用伺服器端轉送。

      1. 因為您已啟用ECID，所以會開始轉送。
   1. 請儘快將您的程式碼從使用者端DIL更新為單一端轉送（這可能在Platform標籤中或頁面上，如上方另一節所述）。

      1. 您應會收到適當的JSON回應，回應 [!DNL Analytics] 信標(請參閱 [驗證和疑難排解](#validation-and-troubleshooting) 區段以取得更多詳細資訊)。


>[!NOTE]
>
>請務必儘可能將這兩個步驟鄰近執行，因為在上面的步驟1和2之間，您會有進入AAM的重複資料。 換言之，單一端轉送已開始從傳送資料 [!DNL Analytics] 至AAM，而且由於DIL程式碼仍在頁面上，也會有直接從頁面進入AAM的點選，從而使資料翻倍。 一旦您將程式碼從DIL更新到伺服器端轉送，就會緩解此問題。

>[!NOTE]
>
>若您想讓資料有小差異，而不是資料有小重複，您可以切換上述步驟1和2的順序。 將程式碼從DIL移動到伺服器端轉送會停止資料流入AAM，直到您能夠翻轉開關以開啟的伺服器端轉送為止。 [!UICONTROL report suite]. 一般而言，客戶寧願將資料增加一倍，也不願錯過將訪客帶入特徵和 [!UICONTROL segments].

#### 擁有許多網站時的移轉時間 [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

本主題在前幾節中會簡單介紹，主要策略可歸納如下：

移轉一個網站/[!UICONTROL report suite] (或網站群組/[!UICONTROL report suites])。

不過，根據幾種可能的情況，這可能會有點棘手：

* 您有一個包含數個相異專案的網站 [!UICONTROL report suites]
* 您有 [!UICONTROL report suite] 包括數個網站(例如全域 [!UICONTROL report suite])
* 您使用一個Platform標籤屬性來涵蓋多個網站
* 您擁有不同網站的不同開發團隊

因為這些專案，可能會變得有點複雜。 我的最佳建議是：

* 請花點時間根據上述說明，制定移轉至伺服器端轉送的策略
* 根據Platform標籤中的單一屬性(或單一 [!DNL AppMeasurement] 檔案)通常會對應至1或2個相異專案 [!UICONTROL report suites]，您應該能制定一個計畫，逐一處理這些不同的群組，將您的企業更新為伺服器端轉送
* 如果您正在與Adobe顧問合作，請和他們討論您的移轉計畫，以便他們視需要提供協助

## 驗證和疑難排解 {#validation-and-troubleshooting}

驗證伺服器端轉送是否正常運作的主要方法是，檢視來自應用程式的任何Adobe Analytics點選回應。

如果您沒有執行來自的資料伺服器端轉送 [!DNL Analytics] 若要Audience Manager，則不會有任何回應 [!DNL Analytics] 信標（除2x2畫素以外）。 不過，如果您正在執行伺服器端轉送，則您可在以下位置確認一些專案： [!DNL Analytics] 要求和回應，讓您知道 [!DNL Analytics] 與Audience Manager正確通訊、轉送點選及取得回應。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>請注意誤報的「Success」。 如果有回應，且一切似乎運作正常，請確定您擁有 `stuff` 回應中的物件。 若未這麼做，您可能會看到一則訊息，指出 `"status":"SUCCESS"`. 雖然聽起來不合理，但這實際上是運作「不」正常的證明。
>
>如果您看到這個訊息，表示您已完成Platform標籤中的程式碼更新，或者 [!DNL AppMeasurement]，但此檔案中的轉送 [!DNL Analytics] [!DNL Admin Console] 尚未完成。 在此情況下，您需要確認已在「 」中啟用伺服器端轉送 [!DNL Analytics] [!DNL Admin Console] 您的 [!UICONTROL report suite]. 如果您已啟用，但尚未滿4小時，請耐心等候，因為在後端進行所有必要的變更可能需要花很長的時間。


![誤判成功](assets/falsesuccess.png)

如需伺服器端轉送的詳細資訊，請參閱 [檔案](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
