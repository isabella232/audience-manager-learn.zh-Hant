---
title: 使用外觀相似的模型將銷售的庫存從第一方資料擴展
description: 在本教程中，我們將介紹設定和使用外觀相似的模型所應採取的步驟，以便您能夠建立新的外觀相似的觀眾，並將它們作為轉換段的擴展銷售。
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

# 使用外觀相似的模型將銷售的庫存從第一方資料擴展 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

在本教程中，我們將介紹您設定和使用「外觀相似」時應採取的步驟 [!UICONTROL Models]，以便您能夠建立新的外觀相似的受眾，並將其作為轉換段的擴展銷售。

## 用例詳細資訊 {#use-case-details}

您是內容發佈者。 如果您已經將站點上轉換器的庫存售罄，則您可能認為您的銷售機會就此結束。 輸AAM入外觀 [!UICONTROL Models]。 通過使用此功能，您可以進一步擴展已售出的庫存量，還可以銷售那些可能尚未轉換過產品但看起來/行為像已轉換產品的人的受眾。 此受眾群通常的售價低於實際的轉換器，但通過為希望在您的網站上投放廣告的廣告商提供額外的受眾選項，您仍可以增加收益。 此使用案例的額外好處是，在您的第一方資料上運行此模型不會花費任何成本。

本教程中的步驟如下：

1. 識別/建立理想的用戶（轉換）特性或段
1. 使用此轉換特性/段作為基本項建立模型
1. 選擇 [!UICONTROL First party] 模型中的資料源並運行模型
1. 建立 [!UICONTROL Algorithmic Trait] 從模型結果中添加特徵到段
1. 向感興趣的廣告商提供該分部以擴大轉換分部銷售

## 識別或建立理想的用戶（轉換）特性或段 {#identify-create-an-ideal-user-conversion-trait-or-segment}

您試圖在您的站點上讓人員執行什麼操作？ 您的轉換事件是什麼？ 當然，根據您的站點類型/垂直和組織目標，有許多不同的答案。 無論如何，為符合這些AAM標準的訪客創造一種特質是很普遍的。

在此使用情形中，已假定這一點，因為您已經為轉換器人員銷售了庫存。 但是，在本教程中，最好將其作為其餘使用案例的參考。

此外，當使用事件來創造特徵時，需要記住一個重要的要點，這樣你就不會收集到更多的用戶來參與這個特性。 請觀看以下視頻，瞭解大的顯示。 :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注：** 在上面的視頻中，我展示的示例假定您有Adobe Analytics。 顯然，情況可能並非如此。 如果您有Google Analytics(GA)，則我們有一個模組，您可以使用它將資料發送AAM到(請參閱 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html))，並且如果您的站點上的轉換活動是由AAMGA發送到的，則可以從中建立轉換特性。 如果您有不同的分析解決方案（或沒有分析解決方案），您仍然可以通過我們的代AAM碼和DIL將資料發送到 `submit` 函式等。 (請參閱 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html))。 然後，再次，基於在站點上執行轉換活動時發送的資料建立轉換特性。

## 從第一方資料建立外觀相似的模型 {#creating-a-look-alike-model-from-first-party-data}

在此步驟中，我們將建立 [!UICONTROL First Party] 長相相似的模型。 這意味著，我們不僅要將第一方轉換特性/段用於基本特性/段（無論如何，這對大多數模型來說都是正常的），而且我們還將只針對更多看起來像轉換器的人查看第一方資料池。 我們不會查看第二方或第三方資料。

在這種使用情形中，這很重要，因為我們試圖在我們的網站上建立一個看起來像轉換器但還沒有轉換的用戶群，這樣我們就可以將這個外觀相似的用戶群賣給感興趣的廣告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 建立算法特徵 {#creating-an-algorithmic-trait}

接下來，我們需要建立 [!UICONTROL Algorithmic Trait]，以便模型的結果可用。 沒有創造特質，模型是無用的。 所以當模型運行後，一定要進入特質對話並建立 [!UICONTROL Algorithmic Trait]。 下面的視頻瀏覽了一下，並顯示了一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 提供 [!UICONTROL Algorithmic Segment] 向廣告商 {#offering-the-algorithmic-segment-to-advertisers}

建立 [!UICONTROL Algorithmic Trait]，可以建立新段以將其放入，以便激活資料(不能激活一個特徵，而是使用 [!UICONTROL Algorithmic Trait] 中，以便激活（使用）段。

一旦您建立了一個在外觀相似的模型中得分較高的第一方訪問者群（即看起來像轉換器但尚未碰巧轉換），即使您已經銷售了網站上所有實際轉換器庫存後，您也可以將這一群提供給站點上的廣告客戶。 這是擴展此受眾並通過使用Look-Lake繼續看到額外收入的絕佳方法 [!UICONTROL Models] Audience Manager。
