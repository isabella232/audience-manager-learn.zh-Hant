---
title: 如何識別您的Audience Manager合作夥伴ID或子網域
description: 實作某些Experience Cloud功能時，您需要知道Audience Manager「合作夥伴ID」是什麼（有時也稱為「客戶ID」或「子網域」）。 在此影片中，我們將向您展示兩個位置，讓您在Audience ManagerUI中取得此ID。
feature: 實作基本資訊
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: 「開發人員、資料工程師」
level: 中級
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---


# 如何識別Audience Manager子域{#how-to-identify-your-audience-manager-partner-id-or-subdomain}

實作某些Experience Cloud功能時，您需要知道Audience Manager`Subdomain`是什麼（有時也稱為`client ID`或`Partner ID`）。 在此影片中，我們將向您展示兩個位置，讓您在Audience ManagerUI中取得此資訊。

## 放棄結束…… {#giving-away-the-ending}

如果您不想看這個短片就直接跳入並找到它，您可以在UI的兩處找到`Partner Subdomain`:

1. 如果您已經建立[!UICONTROL rule-based] [!UICONTROL trait]，請按一下&#x200B;**[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 位於該資 [!UICONTROL trait] 料夾清單 [!UICONTROL traits] 的旁邊，URL會將您的子網域包含在URL中。
1. 如果您進入&#x200B;**[!UICONTROL Tools]** > **[!UICONTROL Tags]**&#x200B;介面，並按一下容器的&#x200B;**[!UICONTROL Get code]**，子網域會接近Akamai行的結尾

如果您無法透過這些快速參考資料快速找到它，則視訊將是您投入的時間。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>為Adobe Experience Cloud的每位客戶指派了數值ID，這通常稱為「PID」或「合作夥伴ID」。 這不是我們在本文和影片中討論的ID。 相反，「合作夥伴子域」（有時稱為夥伴ID）通常是客戶端名稱的版本，是資料所發送到的伺服器的子域。 例如，如果您的公司是「Bob&#39;s Knobs」（當然，哈哈），則您的合作夥伴子網域可能是「bobsknobs」，而「PID」則更像「12345」。 您通常不需要知道您的PID，但您的子網域很重要，因此您可以設定Audience Manager實作。

