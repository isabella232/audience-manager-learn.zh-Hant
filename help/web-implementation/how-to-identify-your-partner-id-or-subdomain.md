---
title: 如何識別您的Audience Manager合作夥伴ID或子網域
description: 實作某些Experience Cloud功能時，您需要知道您的Audience Manager「合作夥伴ID」是什麼（有時也稱為「用戶端ID」或「子網域」）。 在此影片中，我們將向您展示在Audience Manager UI中可取得此ID的兩個位置。
feature: implementation basics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---


# 如何識別您的Audience Manager子網域 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

實作某些Experience Cloud功能時，您需要知道您的Audience Manager `Subdomain` 是什麼(有時也稱為 `client ID` 或 `Partner ID`)。 在此影片中，我們將向您展示兩個位置，讓您在Audience Manager UI中取得此資訊。

## 放棄結局…… {#giving-away-the-ending}

如果您不想看這個短片就直接跳入並尋找，您可以在UI的 `Partner Subdomain` 兩處找到：

1. 如果您已建立，請按 [!UICONTROL rule-based] 一 [!UICONTROL trait]下 **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 位於該資 [!UICONTROL trait] 料夾清單的 [!UICONTROL traits] 旁邊，URL會將您的子網域包含在URL中。
1. 如果您進入 **[!UICONTROL Tools]** >介 **[!UICONTROL Tags]** 面並按一 **[!UICONTROL Get code]** 下容器，子網域即會接近Akamai行的結尾

如果您無法透過這些快速參考資料快速找到它，則視訊將是您投入的時間。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloud的每位客戶都會獲得一個數值ID，這通常稱為「PID」或「合作夥伴ID」。 這不是我們在本文和影片中討論的ID。 相反，「合作夥伴子域」（有時稱為夥伴ID）通常是客戶端名稱的版本，是資料所發送到的伺服器的子域。 例如，如果您的公司是「Bob&#39;s Knobs」（當然，哈哈），則您的合作夥伴子網域可能是「bobsknobs」，而「PID」則更像「12345」。 您通常不需要知道您的PID，但您的子網域很重要，因此您可以設定您的Audience Manager實作。

