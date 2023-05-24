---
title: 使用相似模型，從第一方資料擴充已售出存貨
description: 在本教學課程中，我們將逐步引導您設定和使用相似對象模型的步驟，以便您可以建立新的相似對象，並將其作為轉換區段的擴充功能來銷售。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# 使用相似模型，從第一方資料擴充已售出存貨 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

在本教學課程中，我們將逐步引導您完成設定及使用「相似」應採取的步驟 [!UICONTROL Models]，以建立新的相似受眾，並作為轉換區段的擴充功能來銷售。

## 使用案例詳細資訊 {#use-case-details}

您是內容發行者。 如果您已經將網站上轉換器的庫存售罄，您可能會認為您的機會到此結束。 輸入AAM相似專案 [!UICONTROL Models]. 透過使用此功能，您可以進一步擴充已售出的存貨，並銷售對象為可能尚未轉換的人，但外表或行為與已轉換的人相似。 此受眾區段的售價通常低於實際的轉換者，但還是可以讓您為想在您的網站上放置廣告的廣告商提供額外的受眾選項，以增加您的利潤。 此使用案例的額外優點在於，對第一方資料執行此模型不會產生任何費用。

本教學課程中的步驟如下：

1. 識別/建立理想的使用者（轉換）特徵或區段
1. 使用此轉換特徵/區段作為基本專案來建立模型
1. 選擇 [!UICONTROL First party] 模型中的資料來源並執行模型
1. 建立 [!UICONTROL Algorithmic Trait] 從模型結果新增特徵至區段
1. 將區段提供給感興趣的廣告商，以延長轉換區段的銷售額

## 識別或建立理想的使用者（轉換）特徵或區段 {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想讓訪客在您的網站上執行什麼動作？ 什麼是轉換事件？ 當然，根據您的網站型別/垂直和您的組織目標，此問題有許多不同的答案。 無論如何，在AAM中，為符合這些條件的訪客建立特徵是很常見的。

在此使用案例中，假設已發生這種情況，因為您已針對轉換者將庫存售罄。 不過，就本教學課程而言，最好將其討論為其他使用案例的參考。

此外，使用事件建立特徵時，請牢記以下主要需知，以免將超過應有數量的使用者收集進特徵中。 觀看以下影片以瞭解重大揭露。 ：)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上述影片中，我顯示的範例假設您有Adobe Analytics。 顯然，情況可能並非如此。 如果您有Google Analytics(GA)，我們有一個模組，可用來將資料傳送至AAM (請參閱 [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html))，而如果您網站上的轉換活動是在GA前傳送至AAM，則您可以從中建立轉換特徵。 AAM如果您有其他分析解決方案（或沒有分析解決方案），您仍可透過我們的DIL代碼和 `submit` 函式等 (請參閱 [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html))。 接著，再次根據網站上執行轉換活動時傳送的資料建立轉換特徵。

## 從第一方資料建立相似模型 {#creating-a-look-alike-model-from-first-party-data}

在此步驟中，我們將建立 [!UICONTROL First Party] 相似模型。 這表示我們不僅會對基本特徵/區段使用第一方轉換特徵/區段（反正大多數模型都會這麼做），而且我們只會為看起來像是轉換器的人檢視第一方資料池。 我們不會考慮第二方或第三方資料。

在此使用案例中，這很重要，因為我們正在嘗試建立網站上的使用者區段，這些使用者看起來像是轉換器，但還沒有轉換，以便我們可以將此相似區段銷售給感興趣的廣告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 建立演演算法特徵 {#creating-an-algorithmic-trait}

接下來，我們需要建立 [!UICONTROL Algorithmic Trait]，以便可以使用模型的結果。 如果不建立特徵，模型就毫無用處。 所以在模型執行後，請務必進入特徵對話方塊並建立 [!UICONTROL Algorithmic Trait]. 以下影片會逐步解說，並顯示一些秘訣。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 提供 [!UICONTROL Algorithmic Segment] 至廣告商 {#offering-the-algorithmic-segment-to-advertisers}

建立 [!UICONTROL Algorithmic Trait]，您可以建立新區段來放置它，這樣您就可以啟用資料(您無法啟用特徵，而是要使用建立新的單一特徵區段 [!UICONTROL Algorithmic Trait] 以啟動（使用）區段。

一旦您建立了在相似模型中得分較高的第一方訪客區段（亦即看起來像轉換者，但尚未發生轉換），您就可以將此區段提供給您網站上的廣告商，即使您已售出網站上所有實際轉換者的庫存。 這是擴充此對象的最佳方式，而且能繼續透過使用相似對象來檢視額外收入 [!UICONTROL Models] 在Audience Manager中。
