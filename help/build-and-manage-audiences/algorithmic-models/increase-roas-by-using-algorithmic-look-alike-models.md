---
title: 在Audience Manager中使用演算法（類似）模型來增加ROAS
description: Audience Manager的相似模型真正的威力，是當您針對來自第二方與第三方資料來源的優質、全新的使用者群，尋求擴大基準受眾群時。 在本教學課程中，瞭解從這些資料建立模型的步驟。
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1849
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---


# 在Audience Manager中使用演算法（類似） [!UICONTROL Models] 來增加ROAS {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager的Look-lake的真正力量來自 [!UICONTROL Modeling] 您尋求將基準受眾與來自和新的優質使用者群 [!UICONTROL second party] 體 [!UICONTROL third party][!UICONTROL data sources]。 在本教學課程中，瞭解從這些資料建立應用程式 [!UICONTROL model] 所需的步驟。

## 從Audience [!UICONTROL Second Party] Marketplace啟 [!UICONTROL Third Party] 用或資料串流 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

為了在類似 [!UICONTROL second party] 的 [!UICONTROL third party] 介面中使用和使用資料，我們 [!UICONTROL model]必須先將此資料啟用至您的Audience Manager介面。 Adobe有大量資料供 [!UICONTROL second party] 應 [!UICONTROL third party] 商可供您選擇。 這些功能可在AAM的自助介面中透過Audience Marketplace提供。 導覽至Audience Marketplace並瀏覽各種可能。 以下視訊將告訴您如何做到，包括如何啟用免費的「購買前先試用」串流，以便您鎖定對您的組織最有用的資料，然後再提交資料供應商的價格。

此外，為了協助您研究並決定要使用哪些資料提供者，最佳的資源就是 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 識別／建立理想的使用者（轉換） [!UICONTROL trait] 或 [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想要在您的網站上吸引訪客做什麼？ 您的轉換事件是什麼？ 當然，這個問題有許多不同的答案，視您的網站類型／垂直和組織目標而定。 無論如何，在AAM中，為符合這些條件的訪 [!UICONTROL trait] 客建立訪客是很常見的。

在下面的影片中，我將示範如何建立轉換 [!UICONTROL trait]，當您繼續進行本教學課程並建立類似的轉換時，您會想要建立轉換 [!UICONTROL model]。

此外，當使用Adobe Analytics事件進行建立時 [!UICONTROL traits]，您需要記住一個重大問題，以避免收集到的使用者數量超出您的需求 [!UICONTROL trait]。 觀看以下影片，瞭解大的顯示效果。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上述影片中，我展示的範例假設您有Adobe Analytics。 顯然，情況可能並非如此。 如果您有Google Analytics(GA)，我們有一個模組可讓您用來將資料傳送至AAM(請參閱檔案 [)，如果您網站上的轉換活動是透過GA傳送至AAM，則您可以從中建立轉](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)[!UICONTROL trait] 換。 如果您有不同的分析解決方案（或沒有分析解決方案），您仍可透過我們的DIL程式碼和函式，將資料 `submit` 傳送至AAM，等等。 (請參閱 [檔案](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html))。 然後，根據在 [!UICONTROL trait] 網站上執行轉換活動時傳送的資料來建立轉換。

## 從資料建立類似 [!UICONTROL Model] 的 [!UICONTROL Second Party] 內 [!UICONTROL Third Party] 容 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

完成上述步驟後，我們現在已準備好建立演算法（類似） [!UICONTROL Model]。 在設定時，我們 [!UICONTROL model]會將轉換作為基本 [!UICONTROL trait][!UICONTROL trait][!UICONTROL third party] 訪客（我們想複製的關鍵訪客），而且我們會將啟用的資料串流當做我們的人員庫。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳實踐 {#an-important-best-practice}

當在Audience Manager中建 [!UICONTROL model] 立演算法時，顯然我們希望 [!UICONTROL model] 盡可能有效地執行。 由於 [!UICONTROL model] 考慮的是 [!UICONTROL traits] 您基地 [!UICONTROL trait]/成員的所有成員，[!UICONTROL segment] 如果所有人員都在 [!UICONTROL model] / [!UICONTROL trait][!UICONTROL segment]。 因此，如果您有任何超級通用 [!UICONTROL traits] （如所有造訪您網站的人，或收到您任何廣告的人等），請確定 [!UICONTROL data source] 其所屬的不包含在 [!UICONTROL data sources] 您的中 [!UICONTROL model]。 在本文的使用案例中，您不太可能這麼做，因為我們主要針對我們新的外觀品牌來檢視資料，但無論如何都值得一提，並套用至您的「所有」演算法 [!UICONTROL third party][!UICONTROL models]。

## 建立演算法 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下來，我們需要建立演算法 [!UICONTROL Trait]，以便使用演算 [!UICONTROL model] 結果。 如果不建立 [!UICONTROL trait]模型，則模型是無用的。 因此，在執行 [!UICONTROL model] 後，請務必進入對話方塊並 [!UICONTROL trait] 建立演算法 [!UICONTROL Trait]。 以下視訊將逐步檢視，並顯示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 從數 [!UICONTROL Segment] 據創 [!UICONTROL Model] 建併發送到DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

一旦您建立演算法 [!UICONTROL Trait]，就可以建立新的演算法，以便將其放入，以便您能啟動資料(您無法啟動，而是使用演算法來建立新的單一 [!UICONTROL segment] - [!UICONTROL trait]，以便您能夠啟動（使用）[!UICONTROL trait][!UICONTROL segment][!UICONTROL Trait][!UICONTROL segment])。

一旦您從此演算法 [!UICONTROL segment] 中建立 [!UICONTROL trait]，您就會有潛在客戶的受眾，這些潛在客戶看起來就像您網站上已轉換過的人。 現在，您可以將 [!UICONTROL segment] 它對應至Audience Manager中的任 [!UICONTROL destinations] 何DSP。 您可以將行銷目標鎖定在那些在網站上轉換的訪客，而不只是一般大眾，從而提高廣告支出的回報。 祝你好運！
