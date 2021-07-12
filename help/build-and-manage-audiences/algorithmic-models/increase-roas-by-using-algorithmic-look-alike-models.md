---
title: 在Audience Manager中使用演算法（相似）模型，以增加ROAS
description: Audience Manager相似建模的真正威力，在於您尋求針對來自第二方和第三方資料來源的全新優質使用者集，擴展基準受眾。 在本教學課程中，了解從此資料建立模型的步驟。
feature: 演算法模型
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 0%

---

# 在Audience Manager中使用演算法（相似）[!UICONTROL Models]提高ROAS {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

當您嘗試針對[!UICONTROL second party]和[!UICONTROL third party] [!UICONTROL data sources]中的全新品質使用者集，展開基線對象時，Audience Manager相似[!UICONTROL Modeling]的真正力量就來了。 在本教學課程中，了解從此資料建立[!UICONTROL model]所需的步驟。

## 從Audience Marketplace啟用[!UICONTROL Second Party]或[!UICONTROL Third Party]資料流 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

若要在相似的[!UICONTROL model]中使用[!UICONTROL second party]和[!UICONTROL third party]資料，首先必須在您的Audience Manager介面中啟用此資料。 Adobe有大量[!UICONTROL second party]和[!UICONTROL third party]資料提供者，您可以從中選擇。 這些功能可在AAM的自助介面中，透過Audience Marketplace提供。 導覽至Audience Marketplace並瀏覽各種可能性。 以下影片將示範如何執行此作業，包括如何啟用免費「您購買前試用」資料流，以便您能夠在提交資料提供者的定價之前，鎖定對貴組織最有用的資料。

此外，為了協助您研究並決定要使用哪個資料提供者，最好的資源是[[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 識別/建立理想的使用者（轉換）[!UICONTROL trait]或[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您希望讓使用者在您的網站上執行什麼操作？ 什麼是您的轉換事件？ 當然，此問題有許多不同的答案，視您的網站類型/垂直與組織目標而定。 無論如何，AAM中通常會為符合這些條件的訪客建立[!UICONTROL trait]。

在以下影片中，我將示範如何建立轉換[!UICONTROL trait]，當您繼續閱讀本教學課程並建立相似的[!UICONTROL model]時，將會想要建立此轉換。

此外，使用Adobe Analytics事件建立[!UICONTROL traits]時，請謹記一個重要記號，這樣就不會收集到比收集更多使用者到[!UICONTROL trait]中的情況。 請觀看以下影片以了解大的顯示。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上述影片中，我顯示的範例假設您有Adobe Analytics。顯然，情況可能並非如此。 如果您有Google Analytics(GA)，則我們有一個模組，您可以使用該模組將資料傳送至AAM（請參閱[documentation](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)），而如果您網站上的轉換活動是透過GA傳送至AAM，則您可以從中建立轉換[!UICONTROL trait]。 如果您有不同的分析解決方案（或沒有分析解決方案），您仍可以透過我們的DIL程式碼和`submit`函式等將資料傳入AAM。 （請參閱[檔案](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)）。 然後根據網站上執行轉換活動時傳送的資料，建立轉換[!UICONTROL trait]。

## 從[!UICONTROL Second Party]或[!UICONTROL Third Party]資料建立相似[!UICONTROL Model] {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

完成上述步驟後，我們現在可以建立演算法（相似）[!UICONTROL Model]。 在設定[!UICONTROL model]時，我們將使用轉換[!UICONTROL trait]作為基礎[!UICONTROL trait]（要複製的關鍵訪客），並將使用已啟用的[!UICONTROL third party]資料流作為提取的人員池。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳做法 {#an-important-best-practice}

在Audience Manager中建立演算法[!UICONTROL model]時，顯然我們希望[!UICONTROL model]盡可能有效。 由於[!UICONTROL model]正在考慮基[!UICONTROL trait]/[!UICONTROL segment]成員屬於的所有[!UICONTROL traits]，因此，如果所有人員都位於[!UICONTROL trait]/[!UICONTROL segment]中，則對[!UICONTROL model]沒有幫助。 因此，如果您有任何超通用[!UICONTROL traits]（如前往您網站的每個人，或收到您任何廣告的每個人等），請確定他們所屬的[!UICONTROL data source]未包含在您的[!UICONTROL model]的[!UICONTROL data sources]中。 在本文的使用案例中，您不太可能這麼做，因為我們專注於查看[!UICONTROL third party]資料以取得新的外觀，但無論如何，這都值得一提，並適用於您的演算法[!UICONTROL models]。

## 建立演算法[!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下來，我們需要建立演算法[!UICONTROL Trait]，才能使用[!UICONTROL model]的結果。 若未建立[!UICONTROL trait]，則模型無用。 因此，在[!UICONTROL model]執行後，請務必前往[!UICONTROL trait]對話方塊，並建立演算法[!UICONTROL Trait]。 以下影片會逐步介紹，並提供一些秘訣。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 從[!UICONTROL Model]資料建立[!UICONTROL Segment]並將其傳送至DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

建立演算法[!UICONTROL Trait]後，您可以建立新的[!UICONTROL segment]以便將其放入，以便啟動資料(您無法啟動[!UICONTROL trait]，而是使用演算法[!UICONTROL Trait]建立新的[!UICONTROL trait] [!UICONTROL segment]，以便啟動（使用）[!UICONTROL segment])。

一旦您從此演算法[!UICONTROL trait]建立[!UICONTROL segment]後，即會有一群潛在客戶，他們看起來就像已在您的網站上轉換的人。 現在，您可以將此[!UICONTROL segment]對應至Audience Manager中的任何DSP [!UICONTROL destinations]。 您可以將行銷目標定位為那些在您的網站上轉換的訪客，而非一般大眾，進而提高廣告支出的回報。 祝你好運！
