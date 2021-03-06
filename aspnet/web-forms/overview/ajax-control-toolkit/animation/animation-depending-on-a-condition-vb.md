---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: 依據條件 (VB) 的動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫是否是...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: be43b68ce77819cdcd09d8e875604db90e8a8d96
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833201"
---
<a name="animation-depending-on-a-condition-vb"></a>依據條件 (VB) 的動畫
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫執行時是否也取決於一些 JavaScript 程式碼的表單中的條件。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫執行時是否也取決於一些 JavaScript 程式碼的表單中的條件。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要 `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnLoad>`頁面完全載入後，執行動畫。 而不是一個一般的動畫，`<Condition>`元素派上用場。 提供的值為 JavaScript 程式碼`ConditionScript`屬性會在執行階段執行。 如果評估為 true，動畫會執行，否則不。 下列標記會提供兩個動畫，每個正在執行 50%的隨機時的案例。 因為只能有一張動畫內`<OnLoad>`，這兩個`<Condition>`動畫會聯結在一起使用`<Sequence>`項目：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

請注意，為小於符號 (`<`) 中`ConditionScript`屬性必須逸出 （）。 當您執行此指令碼，可能是沒有動畫執行時，或兩者的其中一個存在，或兩個的作用。


[![面板淡出但不調整大小，因此未在第二個動畫執行後的第一個](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

面板淡出但不調整大小，因此未在第二個動畫執行後的第一個 ([按一下以檢視完整大小的影像](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](executing-several-animations-after-each-other-vb.md)
> [下一頁](picking-one-animation-out-of-a-list-vb.md)
