---
description: เขียนแอพถ่ายรูปแล้วแปลงเป็นข้อความกัน ใช้ได้ทุกภาษา
---

# อ่านลายมือจากรูปเป็นตัวอักษร \(OCR\)

![](../../.gitbook/assets/image-965.png)

ในรอบนี้เราจะลองเขียนโค้ดให้มัน**แปลงข้อความที่อยู่ในรูปให้ออกมาเป็นตัวหนังสือ** หรือที่เราเรียกกันติดปากว่า **`OCR`** `Optical Character Recognition` ดูบ้างนะ ซึ่งการที่จะทำแบบนี้ได้ผมจะใช้ AI สำเร็จรูปของ Microsoft Azure ที่ชื่อว่า **Cognitive Services** ครับ

{% hint style="success" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นหนึ่งในซีรี่ AI ดังนั้นถ้าเพื่อนสนใจของสนุกๆ เช่น [**Login ด้วยใบหน้า**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/faceauth) หรือ **ยืนยันตัวตนด้วยเสียง** [**แปลงภาพเป็นข้อความ**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/ocr) และอื่นๆ สามารถดูเนื้อหาทั้งหมดได้จาก side menu ในหมวดของ **Cognitive Services** ครับ กำลังทำเรื่อยๆอยู่ ส่วนถ้าอยากรู้ว่า AI สำเร็จรูปตัวอื่นๆของ Microsoft Azure มีอะไรน่าเล่นบ้าง ไปอ่านกันได้จากลิงค์นี้เลยครัช [👶 Azure Cognitive Services](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services) เชื่อผมเต๊อะ AI ไม่ได้ยากแบบที่คิด
{% endhint %}

\*\*\*\*

## 😗 ทำความเข้าใจกันก่อน

ตัวโปรแกรมของเราจะทำการส่งรูปไปให้ AI ดู แล้วเจ้า AI จะส่งกลับมาว่าข้อความในรูปมันเขียนว่าอะไรบ้าง ซึ่งตัวอย่างนี้ผมจะไม่ทำ UI เลยเพราะเน้นไปที่เรื่องการใช้ AI อย่างเดียวเท่านั้นครับ

ตัวอย่างนี้ผมจะใช้ภาษา C\# เขียน ดังนั้นใครที่จะทำตามด้วย C\# ให้ลง `Visual Studio Code` และ `.NET Core SDK` ในลิงค์ด้านล่างด้วยถ้ายังไม่มี **ส่วนภาษาอื่นๆก็สามารถทำตามได้เหมือนกัน** เพราะขั้นตอนทั้งหมดเราเรียกใช้ **REST API** เพียงอย่างเดียวเลยครับ

* [Download Visual Studio Code](https://code.visualstudio.com/)
* [Download .NET Core SDK](https://dotnet.microsoft.com/download)

## 🤔 เริ่มยังไงดี ?

ก่อนที่จะเริ่มทำอะไรผมอยากให้เข้าใจตรงกันก่อนว่า ในตัวอย่างนี้เราจะต้องทำอะไรกันบ้างตามนี้เลย

1. ขั้นตอนแรกเราต้องสร้าง **Cognitive Services** เสียก่อน เพื่อที่จะใช้งาน AI สำเร็จรูปได้
2. เข้าไปใน Cognitive Services ที่สร้างไว้ เพื่อ**เอา Key กับ Endpoint มา** ซึ่งเจ้าสองตัวนี้จะเป็นเหมือนรหัสลับในการเข้าใช้งาน AI ของเรานั่นเอง
3. เขียนโค้ดเพื่อเรียกใช้งาน Cognitive Services ในขั้นตอนนี้เราจะต้องส่งรหัสลับเพื่อยืนยันว่าเราสามารถใช้งาน AI ตัวนี้ได้
4. ส่งรูปให้ AI อ่านข้อความกลับมาก็เป็นอันจบ

## 🔥 \(1\) สร้าง Cognitive Services กัน

{% hint style="info" %}
**Azure Portal**  
เนื้อหาตรงจุดนี้จะต้องเข้าไปที่ทำที่เว็บ [https://portal.azure.com](https://portal.azure.com) นี้นะครับ ซึ่งเราต้องสมัครสมาชิกก่อน แต่ถ้าใครยังไม่ได้สมัครก็ไปสมัครให้เรียบร้อยแซ๊ร [\(วิธีสมัครจิ้มตรงนี้\)](https://saladpuk.gitbook.io/learn/cloud/azure101/register)
{% endhint %}

1.หลังจากที่ Login เข้ามาละ ที่เมนูด้านซ้ายมือให้กดที่ **`+ Create a resource`** ซะ ส่วนช่องค้นหาให้พิมพ์คำว่า **`Cognitive Services`** ลงไปแล้วกด Enter ได้เบย

![](../../.gitbook/assets/create-cognitiveservices.png)

2.ถัดมาเขาก็จะบอกรายละเอียดเกี่ยวกับ Cognitive Services ว่ามันคืออะไร จะไปศึกษาลองเล่นต่อได้ยังไง ราคาที่ต้องจ่ายต่อเดือนคิดยังไง บลาๆ ก็ถ้าอ่านจนหนำใจแล้วก็จิ้มปุ่ม **Create** เบาๆไป 1 ทีงับ

![](../../.gitbook/assets/cognitive-info.PNG)

3.ถัดมาก็ใส่รายละเอียดของ Cognitive Services ให้เรียบร้อยซะ แล้วก็กดปุ่ม **`Create`** ได้เลย

| ชื่อ | รายละเอียด |
| :--- | :--- |


| **Name** | ชื่อ Cognitive Services ที่จะสร้าง |
| :--- | :--- |


<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Location</b>
      </th>
      <th style="text-align:left">
        <p>&#xE15;&#xE31;&#xE27; service &#xE19;&#xE35;&#xE49;&#xE08;&#xE30;&#xE2A;&#xE23;&#xE49;&#xE32;&#xE07;&#xE44;&#xE27;&#xE49;&#xE20;&#xE39;&#xE21;&#xE34;&#xE20;&#xE32;&#xE04;&#xE44;&#xE2B;&#xE19;</p>
        <p>&#xE43;&#xE19;&#xE15;&#xE31;&#xE27;&#xE2D;&#xE22;&#xE48;&#xE32;&#xE07;&#xE1C;&#xE21;&#xE40;&#xE25;&#xE48;&#xE19;&#xE43;&#xE19;&#xE44;&#xE17;&#xE22;&#xE01;&#xE47;&#xE40;&#xE25;&#xE37;&#xE2D;&#xE01;&#xE40;&#xE1B;&#xE47;&#xE19;
          Southeast Asia</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Pricing tier</b>
      </th>
      <th style="text-align:left">
        <p>&#xE08;&#xE30;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE02;&#xE32;&#xE40;&#xE01;&#xE47;&#xE1A;&#xE40;&#xE07;&#xE34;&#xE19;&#xE40;&#xE23;&#xE32;&#xE41;&#xE1A;&#xE1A;&#xE44;&#xE2B;&#xE19;
          &#xE43;&#xE19;&#xE15;&#xE2D;&#xE19;&#xE19;&#xE35;&#xE49;&#xE22;&#xE31;&#xE07;&#xE44;&#xE21;&#xE48;&#xE21;&#xE35;&#xE15;&#xE31;&#xE27;&#xE1F;&#xE23;&#xE35;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE25;&#xE48;&#xE19;</p>
        <p>&#xE41;&#xE15;&#xE48;&#xE16;&#xE49;&#xE32;&#xE40;&#xE23;&#xE32;&#xE25;&#xE2D;&#xE07;&#xE40;&#xE2A;&#xE23;&#xE47;&#xE08;&#xE41;&#xE25;&#xE49;&#xE27;&#xE25;&#xE1A;&#xE40;&#xE25;&#xE22;&#xE2A;&#xE34;&#xE49;&#xE19;&#xE40;&#xE14;&#xE37;&#xE2D;&#xE19;&#xE01;&#xE47;&#xE08;&#xE48;&#xE32;&#xE22;&#xE44;&#xE21;&#xE48;&#xE16;&#xE36;&#xE07;
          1 &#xE1A;&#xE32;&#xE17;&#xE04;&#xE23;&#xE31;&#xE1A;</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Resource group</b>
      </th>
      <th style="text-align:left">
        <p>&#xE08;&#xE30;&#xE2A;&#xE23;&#xE49;&#xE32;&#xE07; service &#xE19;&#xE35;&#xE49;&#xE44;&#xE27;&#xE49;&#xE43;&#xE19;
          Resource Group &#xE44;&#xE2B;&#xE19;</p>
        <p>&#xE16;&#xE49;&#xE32;&#xE22;&#xE31;&#xE07;&#xE44;&#xE21;&#xE48;&#xE21;&#xE35;&#xE43;&#xE2B;&#xE49;&#xE01;&#xE14;&#xE1B;&#xE38;&#xE48;&#xE21; <b><code>Create new</code></b> &#xE40;&#xE25;&#xE22;&#xE01;&#xE47;&#xE44;&#xE14;&#xE49;&#xE04;&#xE23;&#xE31;&#xE1A;</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

3.เรียบร้อยครับ ที่เหลือก็แค่รอให้มันสร้าง Cognitive Services จนเสร็จ

![](../../.gitbook/assets/image%20%28405%29%20%281%29.png)

## 🔥 \(2\) เอา Key กับ Endpoint กัน

โดยปรกติเวลาที่เราจะเรียกใช้ **REST API** ของ Cognitive Services เราจะต้องส่งรหัสลับเพื่อยืนยันว่าเราเป็นคนที่มีสิทธิ์ในการใช้งาน AI ตัวนี้จริงๆนะ ซึ่งเจ้ารหัสลับก็คือ **Key** นั่นเอง ส่วนเจ้า **Endpoint** ก็คือตัว API ที่เราจะเรียกไป ดังนั้นหลังจากทำขั้นตอนที่ 1 เสร็จแล้ว เราก็ไปเอาของพวกนั้นกันเลย

1.ที่เมนูด้านซ้ายให้กด **`Resource groups`** แล้วเขาเลือก Resource group ที่เราสร้างไว้จากขั้นตอนที่ 1 ครับ

![](../../.gitbook/assets/select-resourcegroup.png)

2.พอเข้ามาใน Resource group แล้วถัดไปเราก็จะเห็น **`Cognitive Services`** ที่เราสร้างไว้ ให้จิ้มมันอย่างอ่อนโยน

![](../../.gitbook/assets/image%20%28976%29.png)

3.พอเข้ามาเราก็จะเห็น **`Key`** กับ **`Endpoint`** ที่เราตามหาเลย แล้วทำการกด copy ทั้ง 2 ตัวไปเก็บไว้ก่อนครับ

![](../../.gitbook/assets/cognitive-key-n-endpoint.png)

## 🔥 \(3\) เขียนโค้ดกัน

ในขั้นตอนนี้ผมจะสร้างโปรเจคของ C\# ขึ้นมา เพื่อเขียนโค้ดในการสร้างเรียกใช้ Cognitive Services ล่ะนะ ดังนั้นขอแบ่งเป็นหัวข้อย่อยๆนิดหน่อย

### 2.1.สร้างโปรเจค

เริ่มต้นก็เปิด command prompt หรือ terminal ขึ้นมาเลย แล้วใช้คำสั่งด้านล่างในการสร้างโปรเจคได้เลย

```text
dotnet new console -n saladpuk-handwritten-text
```

> `saladpuk-handwritten-text` คือชื่อโปรเจคนะครับ อยากได้ชื่ออื่นก็เปลี่ยนได้เลย

ตอนนี้เราก็จะได้โปรเจคมา 1 ตัวละ ถัดไปก็เข้าไปที่โปรเจคนั้นครับด้วยคำสั่งด้านล่างนี้ \(ใครที่เปลี่ยนชื่อโปรเจคก็ใส่ชื่อเป็นชื่อโปรเจคที่ตัวเองตั้งไว้นะ\)

```text
cd saladpuk-handwritten-text
```

ในตัวอย่างนี้การที่เราจะเรียกใช้ Cognitive Service เราจะต้องทำงานผ่าน REST API ธรรมดานี่แหละ ดังนั้นเพื่อความง่ายผมจะลง library เสริม 2 ตัวเพื่อให้ทำงานกับ REST API และ Json ได้ง่ายขึ้นครับ โดยใช้คำสั่งด้านล่างก็เป็นอันเสร็จสิ้นพิธี

```text
dotnet add package RestSharp --version 106.6.10
dotnet add package Newtonsoft.Json
```

ทดสอบว่าโปรเจคไม่มีปัญหาอะไรด้วยคำสั่งด้านล่าง

```text
dotnet build
```

ซึ่งถ้าไม่มีปัญหาอะไรก็น่าจะได้ผลลัพท์ออกมาแบบนี้

```text
Build succeeded.
    0 Warning(s)
    0 Error(s)
```

คราวนี้ให้เราเปิด **Visual Studio Code** ขึ้นมา โดยใช้คำสั่ง `code .` ภายใน command prompt หรือ terminal หรือจะเปิด Visual Studio Code แล้วเปิด folder มาที่ตัวโปรเจคนี้ก็ได้เหมือนกัน

หลังจากที่ Visual Studio Code เปิดขึ้นมาแล้ว ให้ทำการเปิดไฟล์ `Program.cs` ขึ้นมารอเลยครับ

![](../../.gitbook/assets/image%20%28234%29%20%281%29.png)

{% hint style="info" %}
**Visual Studio Code: C\# Extension**  
ในตัวอย่างที่เป็นสีสวยงาม เพราะผมลง extension 2 ตัวให้กับ Visual Studio Code นะครับ ถ้าสนใจก็กดติดตั้งได้จากลิงค์ด้านล่างเลย

* [C\#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [C\# Extensions](https://marketplace.visualstudio.com/items?itemName=jchannon.csharpextensions)
{% endhint %}

### 2.2.เขียนโค้ดติดต่อ Cognitive Services

ตอนนี้ให้เอาโค้ดด้านล่างไปทับใน `Program.cs` ทั้งหมดเลย ซึ่งเจ้าโค้ดด้านล่างจะเป็นแค่โครงคร่าวๆเท่านั้น

{% code title="Program.cs" %}
```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net;
using Newtonsoft.Json;
using RestSharp;

namespace saladpuk_handwritten_text
{
    class Program
    {
        static string SubscriptionKey = "FACE_SUBSCRIPTION_KEY";
        static string Endpoint = "FACE_ENDPOINT";

        static void Main(string[] args)
        {
            var client = new RestClient(Endpoint);

            // เขียนโค้ดต่อที่นี่
        }

        static RestRequest CreateRestRequest(string resource, object requestBody)
        {
            var request = new RestRequest(resource);
            request.AddHeader("ocp-apim-subscription-key", SubscriptionKey);
            if (requestBody != null)
            {
                request.AddHeader("content-type", "application/json");
                request.AddJsonBody(requestBody);
            }
            return request;
        }
    }
}
```
{% endcode %}

จากโค้ดด้านบนเราจะต้องเอา `Key` กับ `Endpoint` ที่ได้มาจากขั้นตอนที่ 2 เอามาใส่ไว้ในบรรทัดที่ 13 กับ 14 เพื่อเตรียมให้เราสามารถเรียก Cognitive Services ได้นั่นเอง ดังนั้นก็ใส่ลงไปครับ

{% code title="Program.cs" %}
```csharp
static string SubscriptionKey = "be6b8fd9fe2a48b5a050f1016b571170";
static string Endpoint = "https://southeastasia.api.cognitive.microsoft.com/";
```
{% endcode %}

{% hint style="danger" %}
**คำเตือน**  
ถ้าใช้ **Key** กับ **Endpoint** จากตัวอย่างของผมมันจะทำงานไม่ได้นะครับ เพราะผมลบของพวกนี้ไปแล้ว ให้ไปสร้างมาเล่นเองนะ
{% endhint %}

## 🔥 \(4\) ส่งรูปให้ AI เปลี่ยนเป็น Text

ถัดมาเราก็จะส่งรูปภาพของเราไปให้กับ **Cognitive Services API** แล้วนะครับ โดยรูปที่จะส่งไปคือรูปนี้

![](../../.gitbook/assets/image%20%28920%29.png)

คราวนี้ API ที่เราต้องใช้ตัวแรกคือนี้ครัช

```javascript
POST https://southeastasia.api.cognitive.microsoft.com/vision/v2.1/read/core/asyncBatchAnalyze HTTP/1.1
Host: southeastasia.api.cognitive.microsoft.com
Content-Type: application/json
Ocp-Apim-Subscription-Key: ••••

{"url":"https://jooinn.com/images/handwritten-text-1.jpg"}
```

ดังนั้นเราก็ลงมือแก้โค้ด C\# กันเบย ก็จะได้โค้ดออกมาหน้าตาราวๆนี้

{% code title="Program.cs" %}
```csharp
static void Main(string[] args)
{
    var client = new RestClient(Endpoint);

    // เรียกใช้ Batch Read File
    Console.WriteLine("Running Batch read file.");
    var operationlocation = string.Empty;
    var batchReadFileRequest = CreateRestRequest("vision/v2.1/read/core/asyncBatchAnalyze", new
    {
        url = "https://jooinn.com/images/handwritten-text-1.jpg"
    });
    var batchReadFileResult = client.Execute(batchReadFileRequest, Method.POST);
    if (batchReadFileResult.StatusCode == HttpStatusCode.Accepted)
    {
        var operationlocationUrl = batchReadFileResult.Headers.FirstOrDefault(it => it.Name == "Operation-Location").Value.ToString();
        operationlocation = Path.GetFileName(operationlocationUrl);
        Console.WriteLine($"-> Done. Operation-Location: {operationlocation}");
    }
    else
    {
        Console.WriteLine($"Error: {batchReadFileResult.Content}");
        return;
    }

    // โค้ดตัวต่อไปเอามาใส่ตรงนี้
}
```
{% endcode %}

จากโค้ดด้านบนมันก็จะเรียก REST API ไปตามที่ผมได้บอกไป ซึ่งสิ่งที่เราจะได้กลับมามันจะอยู่ใน Header ที่ชื่อ **`Operation-Location`** นะครับมีหน้าตาตามด้านล่างเลย ดังนั้นโค้ดเราก็จะอ่านค่า header แล้วเอามาเก็บไว้ในตัวแปรที่ชื่อ `operationlocation` ที่อยู่ในโค้ดด้านบนตรงบรรทัดที่ 16 ครับ

```text
Pragma: no-cache
Operation-Location: https://southeastasia.api.cognitive.microsoft.com/vision/v2.1/read/operations/dcb2e8df-a7d6-4527-b656-c3f6c87d53bc
apim-request-id: 523b53fb-aef3-41c9-9a66-de29313cda86
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Cache-Control: no-cache
Date: Sun, 08 Sep 2019 10:07:04 GMT
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
Content-Length: 0
Expires: -1
```

ถัดไปเราก็จะไปเอาผลลัพท์ของเรากลับมาครับ ว่าเจ้ารู้ที่ส่งไปให้มันมีตัวหนังสืออะไรอยู่ในนั้นบ้าง ด้วยการเรียก API ตัวนี้ครับ

```javascript
GET https://southeastasia.api.cognitive.microsoft.com/vision/v2.1/read/operations/{operationId} HTTP/1.1
Host: southeastasia.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: ••••
```

ซึ่งเจ้า API ตัวนี้ เราจะต้องส่ง **`OperationId`** ที่ได้จากขั้นตอนก่อนหน้าไปให้มันด้วยครับ ดังนั้นผมก็จะเพิ่มโค้ดตัวนี้เข้าไป

{% code title="Program.cs" %}
```csharp
static void Main(string[] args)
{
    ...

    // เรียกใช้ Get Read Operation Result
    Console.WriteLine("Running Get read operation result.");
    while (true)
    {
        var getReadOperationRequest = CreateRestRequest($"vision/v2.1/read/operations/{operationlocation}", null);
        var getReadOperationResult = client.Execute(getReadOperationRequest, Method.GET);
        if (getReadOperationResult.StatusCode == HttpStatusCode.OK)
        {
            const string Succeeded = "Succeeded";
            var result = JsonConvert.DeserializeObject<ReadOperationResultInfo>(getReadOperationResult.Content);
            if (result.Status == Succeeded)
            {
                var linesQry = result.RecognitionResults.SelectMany(it => it.Lines).Select(it => it.Text);
                var message = string.Join(Environment.NewLine, linesQry);
                Console.WriteLine($"-> Done. the message is{Environment.NewLine}{message}");
                break;
            }
        }
        else
        {
            Console.WriteLine($"Incomplete or error, retrying in 1 second.");
            System.Threading.Thread.Sleep(1000);
        }
    }
}

class ReadOperationResultInfo
{
    public string Status { get; set; }
    public IEnumerable<RecognitionInfo> RecognitionResults { get; set; }
}
class RecognitionInfo
{
    public IEnumerable<LineInfo> Lines { get; set; }
}
class LineInfo
{
    public string Text { get; set; }
}
```
{% endcode %}

เจ้า API ตัวนี้มันจะตอบผลลัพท์ข้อความที่มันอ่านจากรูปกลับมาเป็น Json array ยาวๆแยกตามแต่ละบรรทัดเลยครับ เลยทำให้ผมต้องสร้าง model ในบรรทัดที่ 31-43 มารับมันไม่งั้นโค้ดจะอ่านยากครับ และเจ้า API ตัวนนี้บางทีมันก็อาจจะยังวิเคราะห์รูปไม่เสร็จ เราเลยต้องเขียนติด loop เพื่อหน่วงเวลามันนิดหน่อยครับ ในบรรทัดที่ 7-28

แล้วเราก็ลอง Run โปรแกรมโดยการกด `CTRL + F5` หรือจะใช้คำสั่ง `dotnet run` ใน command prompt หรือ terminal ก็ได้ครับ แล้วเราก็จะเห็นผลลัพท์ตามนี้

```text
Running Batch read file.
-> Done. Operation-Location: dbb17070-009a-48fc-8abd-744e75fc9ba0
Running Get read operation result.
-> Done. the message is
We Start With Good
Because all businesses should
be doing something
good.
```

ผลลัพท์อยู่บรรทัดที่ 5-8 ไหนลองเอามาเทียบกับในรูปดูดิ๊

![](../../.gitbook/assets/image%20%28920%29.png)

ตรงกันหมด ก็เป็นอันเรียบร้อยแล้วครับ

## 🤔 ยาวจังของโค้ดทั้งหมดหน่อย

พอดีเนื้อหาค่อนข้างยาวดังนั้นผมขอยก source code ให้ไปดาวโหลดมาลองเล่นเลยดีกว่าครับ

{% file src="../../.gitbook/assets/saladpuk-handwritten-text.zip" caption="Source code: OCR" %}

## 🎯 บทสรุป

ในการทำงานกับ AI จริงๆไม่ใช่เรื่องยากเลยหัวใจหลักของมันจริงๆก็คือการเรียกใช้ **REST API** ให้ถูกตัวเท่านั้น ดังนั้นไม่ว่าเราจะเขียนภาษาไหนก็ตาม เราก็สามารถเรียกใช้ **Cognitive Services** เพื่อทำของประมาณนี้ได้เลย

{% hint style="success" %}
**Cognitive Services API**  
หากใครสนใจอยากดู API ทั้งหมดที่ Microsoft เตรียมไว้ให้เราเรียกใช้ AI สำเร็จรูป ก็สามารถเข้าไปดูได้จากลิงค์ด้านล่างนี้เลยครับ

* [Microsoft Cognitive Services: Computer Vision v.2.1](https://southeastasia.dev.cognitive.microsoft.com/docs/services/5cd27ec07268f6c679a3e641/operations/56f91f2e778daf14a499f21b)
* [Microsoft Cognitive Services APIs](https://southeastasia.dev.cognitive.microsoft.com/docs/services/)
{% endhint %}

{% hint style="success" %}
**Cognitive Services Library**  
สำหรับคนที่ต้องการเขียนทำงานกับ Cognitive Services จริงๆไม่ต้องไปนั่งเขียนเชื่อม API ทีละตัวก็ได้นะ เพราะทาง Microsoft นั้นได้มี Library ให้เราสามารถเรียกใช้ได้เลยครับ เช่นในฝั่ง .NET ก็จะมีตัวนี้ **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** ที่สามารถติดตั้งแล้วใช้งานได้เลย
{% endhint %}

