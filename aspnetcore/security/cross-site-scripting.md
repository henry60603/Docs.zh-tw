---
title: "防止跨網站指令碼"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 95790927-2bfe-445e-b1fd-429c2c7030ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: 1816977837efd82f374a03d9f776db21358e2850
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="46732-103">防止跨網站指令碼</span><span class="sxs-lookup"><span data-stu-id="46732-103">Preventing Cross-Site Scripting</span></span>

<a name=security-cross-site-scripting></a>

<span data-ttu-id="46732-104">跨網站指令碼 (XSS) 是可讓攻擊者將用戶端指令碼 (通常是 JavaScript) 放入網頁的安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="46732-104">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="46732-105">當其他使用者載入受影響的頁面，攻擊者指令碼執行時，讓攻擊者竊取 cookie 和工作階段權杖變更透過 DOM 操作的 web 網頁內容，或瀏覽器重新導向至另一個頁面。</span><span class="sxs-lookup"><span data-stu-id="46732-105">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="46732-106">應用程式會接受使用者輸入，並將它在網頁輸出而不需要驗證、 編碼或逸出它時，通常會發生 XSS 弱點。</span><span class="sxs-lookup"><span data-stu-id="46732-106">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="46732-107">保護您的應用程式對 XSS</span><span class="sxs-lookup"><span data-stu-id="46732-107">Protecting your application against XSS</span></span>

<span data-ttu-id="46732-108">在基本層級 XSS 運作方式是欺騙您的應用程式到插入`<script>`標記成轉譯的頁面上，或是藉由插入`On*`事件的項目。</span><span class="sxs-lookup"><span data-stu-id="46732-108">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="46732-109">若要避免引進 XSS 到他們的應用程式，開發人員應該使用下列預防步驟。</span><span class="sxs-lookup"><span data-stu-id="46732-109">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="46732-110">除非您是遵循下列步驟的其餘部分，永遠不會放入您的 HTML 輸入未受信任的資料。</span><span class="sxs-lookup"><span data-stu-id="46732-110">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="46732-111">不受信任的資料是任何資料可能受攻擊者、 HTML 表單的輸入、 查詢字串、 HTTP 標頭，即使資料源自攻擊者可能會違反您的資料庫，即使它們不能違反您的應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="46732-111">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="46732-112">HTML 項目內有不受信任的資料之前，請先確定它是 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="46732-112">Before putting untrusted data inside an HTML element ensure it is HTML encoded.</span></span> <span data-ttu-id="46732-113">例如 HTML 編碼方式會採用字元&lt;變成安全的表單和&amp;lt;</span><span class="sxs-lookup"><span data-stu-id="46732-113">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="46732-114">請先將不受信任的資料放入 HTML 屬性確認它已編碼的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="46732-114">Before putting untrusted data into an HTML attribute ensure it is HTML attribute encoded.</span></span> <span data-ttu-id="46732-115">HTML 屬性編碼為 HTML 編碼的超集，例如將額外的字元編碼成"和 '。</span><span class="sxs-lookup"><span data-stu-id="46732-115">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="46732-116">之前將不受信任的資料放入 JavaScript 將資料放在 HTML 項目您在執行階段擷取其內容。</span><span class="sxs-lookup"><span data-stu-id="46732-116">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="46732-117">如果這不可能，請確定資料是 JavaScript 編碼。</span><span class="sxs-lookup"><span data-stu-id="46732-117">If this is not possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="46732-118">JavaScript 編碼危險的字元所需的 JavaScript，並將它們取代為其十六進位，例如&lt;會編碼為`\u003C`。</span><span class="sxs-lookup"><span data-stu-id="46732-118">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="46732-119">之前將不受信任的資料放入 URL 查詢字串，請確定它是 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="46732-119">Before putting untrusted data into a URL query string ensure it is URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="46732-120">使用 Razor 的 HTML 編碼方式</span><span class="sxs-lookup"><span data-stu-id="46732-120">HTML Encoding using Razor</span></span>

<span data-ttu-id="46732-121">自動使用 MVC Razor 引擎會將編碼所有輸出來自於變數，除非您真的硬碟以防止它執行這項作業。</span><span class="sxs-lookup"><span data-stu-id="46732-121">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="46732-122">它會使用 HTML 屬性編碼規則，每當您使用 *@* 指示詞。</span><span class="sxs-lookup"><span data-stu-id="46732-122">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="46732-123">為 HTML 屬性編碼為 HTML 編碼這表示您不必擔心自己是否應使用 HTML 編碼或 HTML 屬性編碼的超集。</span><span class="sxs-lookup"><span data-stu-id="46732-123">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="46732-124">您必須確定您只使用在 HTML 內容中，不會在嘗試直接插入 JavaScript 不受信任的輸入。</span><span class="sxs-lookup"><span data-stu-id="46732-124">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="46732-125">標記協助程式也會編碼的輸入您在標記參數中使用。</span><span class="sxs-lookup"><span data-stu-id="46732-125">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="46732-126">採取下列 Razor 檢視;</span><span class="sxs-lookup"><span data-stu-id="46732-126">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="46732-127">此檢視輸出的內容*untrustedInput*變數。</span><span class="sxs-lookup"><span data-stu-id="46732-127">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="46732-128">此變數包含中 XSS 攻擊，也就是使用某些字元&lt;，"和&gt;。</span><span class="sxs-lookup"><span data-stu-id="46732-128">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="46732-129">檢查來源顯示轉譯的輸出編碼為：</span><span class="sxs-lookup"><span data-stu-id="46732-129">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="46732-130">ASP.NET Core MVC 提供`HtmlString`類別可在輸出時不會自動編碼。</span><span class="sxs-lookup"><span data-stu-id="46732-130">ASP.NET Core MVC provides an `HtmlString` class which is not automatically encoded upon output.</span></span> <span data-ttu-id="46732-131">這應該永遠不會用於搭配信任的輸入這將會公開 （expose） XSS 的安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="46732-131">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="46732-132">使用 Razor Javascript 編碼</span><span class="sxs-lookup"><span data-stu-id="46732-132">Javascript Encoding using Razor</span></span>

<span data-ttu-id="46732-133">有時候可能想要將值插入您的檢視中要處理的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="46732-133">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="46732-134">執行這項作業的方法有兩種。</span><span class="sxs-lookup"><span data-stu-id="46732-134">There are two ways to do this.</span></span> <span data-ttu-id="46732-135">將值放在資料屬性中，並擷取在 JavaScript 程式是標記的最安全的方式，可將簡單的值。</span><span class="sxs-lookup"><span data-stu-id="46732-135">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="46732-136">例如: </span><span class="sxs-lookup"><span data-stu-id="46732-136">For example:</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="46732-137">這會產生下列 HTML</span><span class="sxs-lookup"><span data-stu-id="46732-137">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="46732-138">其中，執行時，會轉譯下列工作;</span><span class="sxs-lookup"><span data-stu-id="46732-138">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="46732-139">您也可以直接呼叫 JavaScript 編碼器</span><span class="sxs-lookup"><span data-stu-id="46732-139">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="46732-140">這將瀏覽器中轉換，如下所示。</span><span class="sxs-lookup"><span data-stu-id="46732-140">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="46732-141">請勿串連在 JavaScript 中建立 DOM 項目，不受信任的輸入。</span><span class="sxs-lookup"><span data-stu-id="46732-141">Do not concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="46732-142">您應該使用`createElement()`適當例如指派屬性值和`node.TextContent=`，或使用`element.SetAttribute()` / `element[attribute]=`否則 exception-declaration DOM 式 XSS。</span><span class="sxs-lookup"><span data-stu-id="46732-142">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="46732-143">存取程式碼中的編碼器</span><span class="sxs-lookup"><span data-stu-id="46732-143">Accessing encoders in code</span></span>

<span data-ttu-id="46732-144">HTML、 JavaScript 和 URL 編碼器可以使用您的程式碼有兩種，您可以將它們透過插入[相依性插入](../fundamentals/dependency-injection.md#fundamentals-dependency-injection)或者您可以使用中所包含的預設編碼器`System.Text.Encodings.Web`命名空間。</span><span class="sxs-lookup"><span data-stu-id="46732-144">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="46732-145">如果您使用的預設編碼器，然後在套用至字元範圍的任何處理為安全不會生效-預設編碼器使用最安全的編碼規則可能。</span><span class="sxs-lookup"><span data-stu-id="46732-145">If you use the default encoders then any  you applied to character ranges to be treated as safe will not take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="46732-146">若要使用的可設定的編碼器，透過 DI 您建構函式應該採用*HtmlEncoder*， *JavaScriptEncoder*和*UrlEncoder*適當的參數。</span><span class="sxs-lookup"><span data-stu-id="46732-146">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="46732-147">例如，</span><span class="sxs-lookup"><span data-stu-id="46732-147">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="46732-148">編碼的 URL 參數</span><span class="sxs-lookup"><span data-stu-id="46732-148">Encoding URL Parameters</span></span>

<span data-ttu-id="46732-149">如果您想要建立具有不受信任的輸入，當作值使用的 URL 查詢字串`UrlEncoder`来編碼的值。</span><span class="sxs-lookup"><span data-stu-id="46732-149">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="46732-150">例如：</span><span class="sxs-lookup"><span data-stu-id="46732-150">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="46732-151">變數將包含編碼 encodedValue 之後`%22Quoted%20Value%20with%20spaces%20and%20%26%22`。</span><span class="sxs-lookup"><span data-stu-id="46732-151">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="46732-152">空格、 引號、 標點符號和其他 unsafe 字元將百分比編碼成其十六進位值，例如空格字元會變成 %20。</span><span class="sxs-lookup"><span data-stu-id="46732-152">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="46732-153">請勿使用不受信任的輸入的 URL 路徑的一部分。</span><span class="sxs-lookup"><span data-stu-id="46732-153">Do not use untrusted input as part of a URL path.</span></span> <span data-ttu-id="46732-154">一律為查詢字串值傳遞未受信任的輸入。</span><span class="sxs-lookup"><span data-stu-id="46732-154">Always pass untrusted input as a query string value.</span></span>

<a name=security-cross-site-scripting-customization></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="46732-155">自訂編碼器</span><span class="sxs-lookup"><span data-stu-id="46732-155">Customizing the Encoders</span></span>

<span data-ttu-id="46732-156">根據預設編碼器使用安全清單限制為基本的拉丁 Unicode 範圍，而且其字元程式碼的對等用法為超出該範圍的所有字元都編碼。</span><span class="sxs-lookup"><span data-stu-id="46732-156">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="46732-157">此行為也會影響 Razor TagHelper 和 HtmlHelper 轉譯，因為它會使用編碼器來輸出字串。</span><span class="sxs-lookup"><span data-stu-id="46732-157">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="46732-158">這背後的原因是防範未知或未來的瀏覽器 （上一個瀏覽器 bug 有向上處理非英文字元為基礎的剖析錯誤） 的 bug。</span><span class="sxs-lookup"><span data-stu-id="46732-158">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="46732-159">如果您的網站可讓大量使用非拉丁字元，例如，中文文 （斯拉夫） 或其他人這不可能是您想要的行為。</span><span class="sxs-lookup"><span data-stu-id="46732-159">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="46732-160">您可以自訂編碼器安全清單，以包含範圍中適合您的應用程式在啟動期間，Unicode `ConfigureServices()`。</span><span class="sxs-lookup"><span data-stu-id="46732-160">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="46732-161">例如，使用預設組態，您可能會使用 Razor HtmlHelper 如下所示。</span><span class="sxs-lookup"><span data-stu-id="46732-161">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="46732-162">當您檢視網頁的來源時您會看到它已呈現，如下所示，以中文文字編碼。</span><span class="sxs-lookup"><span data-stu-id="46732-162">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="46732-163">若要擴大範圍的字元視為安全編碼器插入將下列行插入`ConfigureServices()`方法中的`startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="46732-163">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="46732-164">這個範例可擴展包含 Unicode 範圍 CjkUnifiedIdeographs 安全的清單。</span><span class="sxs-lookup"><span data-stu-id="46732-164">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="46732-165">現在就會變成轉譯的輸出</span><span class="sxs-lookup"><span data-stu-id="46732-165">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="46732-166">安全清單的範圍已指定為 Unicode 字碼表不語言。</span><span class="sxs-lookup"><span data-stu-id="46732-166">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="46732-167">[Unicode 標準](http://unicode.org/)有一份[程式碼圖表](http://www.unicode.org/charts/index.html)可用來尋找圖表，其中包含您的字元。</span><span class="sxs-lookup"><span data-stu-id="46732-167">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="46732-168">每個編碼器 Html、 JavaScript 和 Url，必須個別設定。</span><span class="sxs-lookup"><span data-stu-id="46732-168">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="46732-169">自訂安全清單只會影響來源 DI 透過編碼器。</span><span class="sxs-lookup"><span data-stu-id="46732-169">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="46732-170">如果您直接存取透過編碼器`System.Text.Encodings.Web.*Encoder.Default`然後預設值、 基本拉丁文只安全清單將會使用。</span><span class="sxs-lookup"><span data-stu-id="46732-170">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="46732-171">編碼的 take 應放置的位置？</span><span class="sxs-lookup"><span data-stu-id="46732-171">Where should encoding take place?</span></span>

<span data-ttu-id="46732-172">一般會接受作法是編碼發生在輸出，而編碼的值應該永遠不會儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="46732-172">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="46732-173">在輸出的編碼方式，可讓您變更使用的資料，例如，從查詢字串值的 HTML。</span><span class="sxs-lookup"><span data-stu-id="46732-173">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="46732-174">它也可讓您輕鬆地搜尋您的資料，而不需要將值編碼搜尋之前，並可讓您充分任何的利用變更或編碼器所作的 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="46732-174">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="46732-175">做為 XSS 防護技巧的驗證</span><span class="sxs-lookup"><span data-stu-id="46732-175">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="46732-176">驗證是相當有用的工具，在限制 XSS 攻擊。</span><span class="sxs-lookup"><span data-stu-id="46732-176">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="46732-177">例如，包含只有字元 0-9 的簡單數值字串不會觸發 XSS 攻擊。</span><span class="sxs-lookup"><span data-stu-id="46732-177">For example, a simple numeric string containing only the characters 0-9 will not trigger an XSS attack.</span></span> <span data-ttu-id="46732-178">驗證變得更複雜的是如果您想要接受使用者輸入層中的 HTML 剖析 HTML 輸入是很困難，不可能。</span><span class="sxs-lookup"><span data-stu-id="46732-178">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="46732-179">MarkDown 和其他的文字格式將豐富的輸入更安全的選項。</span><span class="sxs-lookup"><span data-stu-id="46732-179">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="46732-180">您永遠不應該依賴單獨的驗證。</span><span class="sxs-lookup"><span data-stu-id="46732-180">You should never rely on validation alone.</span></span> <span data-ttu-id="46732-181">不受信任的輸入，輸出前先編碼，無論何種驗證您已經執行。</span><span class="sxs-lookup"><span data-stu-id="46732-181">Always encode untrusted input before output, no matter what validation you have performed.</span></span>