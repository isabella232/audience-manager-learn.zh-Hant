---
title: 如何識別您的Audience Manager合作夥伴ID或子網域
description: 實作某些Experience Cloud功能時，您必須知道您的Audience Manager「合作夥伴ID」是什麼（有時也稱為「用戶端ID」或「子網域」）。 此影片會在兩處顯示您可在Audience ManagerUI中取得此ID的位置。
feature: 實作基本知識
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 如何識別您的Audience Manager子網域 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

實作某些Experience Cloud功能時，您需要知道您的Audience Manager`Subdomain`是什麼（有時也稱為`client ID`或`Partner ID`）。 此影片會顯示可在Audience ManagerUI中取得此資訊的兩個位置。

## 放棄結局…… {#giving-away-the-ending}

如果您不想只是跳入並在未觀看此短片的情況下尋找，您可以在UI的兩個位置找到您的`Partner Subdomain`:

1. 如果已建立[!UICONTROL rule-based] [!UICONTROL trait]，請按一下&#x200B;**[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 位於該資 [!UICONTROL trait] 料夾清單 [!UICONTROL traits] 的旁邊，URL會在URL中包含您的子網域。
1. 如果您前往&#x200B;**[!UICONTROL Tools]** > **[!UICONTROL Tags]**&#x200B;介面，然後按一下容器的&#x200B;**[!UICONTROL Get code]**，則子網域會朝Akamai行的結尾

如果您無法透過這些快速參考快速找到，影片將是短暫的投稿時間。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>系統會為Adobe Experience Cloud的每個客戶指派數值ID，這通常稱為「PID」或合作夥伴ID。 這不是本文和影片中說的ID。 相反地，「合作夥伴子網域」（有時稱為合作夥伴ID）通常是用戶端名稱的版本，且是資料所要傳送到之伺服器的子網域。 例如，如果您的公司是「Bob&#39;s Knobs」（當然，哈哈），則您的合作夥伴子網域可能是「bobsknobs」，而「PID」則更像「12345」。 您通常不需要知道您的PID，但子網域必須知道，這樣就能設定Audience Manager實施。

