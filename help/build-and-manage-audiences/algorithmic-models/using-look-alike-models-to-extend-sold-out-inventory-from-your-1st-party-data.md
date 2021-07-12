---
title: 使用相似模型來延伸來自第一方資料的已售庫存
description: 在本教學課程中，我們將逐步說明設定和使用相似模型時應採取的步驟，讓您能夠建立新的相似對象，並將它們作為轉換區段的擴充功能銷售。
feature: 演算法模型
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 0%

---

# 使用相似[!UICONTROL Models]從您的[!UICONTROL First Party]資料延長銷貨庫存 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

在本教學課程中，我們將逐步說明設定和使用相似[!UICONTROL Models]所應採取的步驟，讓您能夠建立新的相似對象，並將它們作為轉換的擴充功能銷售[!UICONTROL segment]。

## 使用案例詳細資訊 {#use-case-details}

您是內容發行者。 如果您的網站上已售出轉換器的庫存，您可能會認為您的機會已結束。 輸入AAM相似[!UICONTROL Models]。 透過使用此功能，您可以進一步延長已售出的庫存，並銷售可能尚未轉換但看起來/像是已轉換之人的受眾。 此受眾[!UICONTROL segment]的售價通常低於實際轉換率，但仍可讓您為想在您網站上放置廣告的廣告商提供額外受眾選項，以增加收益。 此使用案例的額外好處是，在第一方資料上執行此模型不需支付任何費用。

本教學課程中的步驟如下：

1. 識別/建立理想的使用者（轉換）[!UICONTROL trait]或[!UICONTROL segment]
1. 使用此轉換建立[!UICONTROL trait]/[!UICONTROL segment]作為基項的[!UICONTROL model]
1. 在[!UICONTROL model]中選擇[!UICONTROL First party]資料源，然後運行[!UICONTROL model]
1. 從[!UICONTROL model]結果建立演算法[!UICONTROL Trait]，並將[!UICONTROL trait]新增至[!UICONTROL segment]
1. 將[!UICONTROL segment]提供給感興趣的廣告商，以擴展轉換[!UICONTROL segment]銷售

## 識別/建立理想的使用者（轉換）[!UICONTROL trait]或[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您希望讓使用者在您的網站上執行什麼操作？ 什麼是您的轉換事件？ 當然，此問題有許多不同的答案，視您的網站類型/垂直與組織目標而定。 無論如何，AAM中通常會為符合這些條件的訪客建立[!UICONTROL trait]。

在此使用案例中，已假設為此，因為您已將庫存售罄給已轉換的人員。 不過，在本教學課程中，建議您參考其他使用案例加以討論。

此外，使用事件來建立[!UICONTROL traits]時，請謹記一個重大問題，這樣就不會收集到比收集更多使用者到[!UICONTROL trait]中的情況。 請觀看以下影片以了解大的顯示。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上述影片中，我顯示的範例假設您有Adobe Analytics。顯然，情況可能並非如此。 如果您有Google Analytics(GA)，則我們有一個模組，您可使用該模組將資料傳送至AAM（請參閱[documentation](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)），而如果您網站上的轉換活動是透過GA傳送至AAM，則您可以從中建立轉換特徵。 如果您有不同的分析解決方案（或沒有分析解決方案），您仍可以透過我們的DIL程式碼和`submit`函式等將資料傳入AAM。 （請參閱[檔案](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)）。 然後，再次根據網站上執行轉換活動時傳送的資料建立轉換特徵。

## 從[!UICONTROL First Party]資料建立相似[!UICONTROL Model] {#creating-a-look-alike-model-from-first-party-data}

在此步驟中，我們將建立[!UICONTROL First Party]相似[!UICONTROL Model]。 這表示我們不僅要將[!UICONTROL first party]轉換[!UICONTROL trait]/[!UICONTROL segment]用於基礎[!UICONTROL trait]/[!UICONTROL segment]（這無論如何對大多數[!UICONTROL models]來說都是正常的），而且我們只會針對更多看起來像轉換器的人，查看[!UICONTROL first party]資料池。 我們不會查看[!UICONTROL second party]或[!UICONTROL third party]資料。

在此使用案例中，這很重要，因為我們正嘗試在網站上建立[!UICONTROL segment]使用者，這些使用者看起來像是已轉換但尚未轉換，因此我們可以將此相似的[!UICONTROL segment]銷售給感興趣的廣告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 建立演算法[!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下來，我們需要建立演算法[!UICONTROL Trait]，以便使用[!UICONTROL model]的結果。 若未建立[!UICONTROL trait],[!UICONTROL model]則無用。 因此，在[!UICONTROL model]執行後，請務必前往[!UICONTROL trait]對話方塊，並建立演算法[!UICONTROL Trait]。 以下影片會逐步介紹，並提供一些秘訣。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 將演算法[!UICONTROL Segment]提供給廣告商 {#offering-the-algorithmic-segment-to-advertisers}

建立演算法[!UICONTROL Trait]後，您可以建立新的[!UICONTROL segment]以便將其放入，以便啟動資料(您無法啟動[!UICONTROL trait]，而是使用演算法[!UICONTROL Trait]建立新的[!UICONTROL trait] [!UICONTROL segment]，以便啟動（使用）[!UICONTROL segment]。

一旦您建立[!UICONTROL segment]的[!UICONTROL first party]訪客在相似[!UICONTROL model]中獲得高分（亦即看起來像是已轉換但尚未發生轉換），即使您已售罄網站上實際轉換的所有詳細目錄，您仍可以將此[!UICONTROL segment]提供給網站上的廣告商。 這是延伸此受眾並繼續查看額外收入的絕佳方式，方法是在Audience Manager中使用相似[!UICONTROL Models]。
