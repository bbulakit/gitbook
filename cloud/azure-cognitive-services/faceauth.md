---
description: บอกลาการ login แบบเดิมๆไปได้เลย Face authentication มาแบ๊ว!
---

# การ Login ด้วยใบหน้า

ในรอบนี้เราจะมาลองใช้ AI สำเร็จรูปของ Microsoft Azure เพื่อทำระบบ Login ด้วยใบหน้ากันดูบ้างว่าเขาทำยังไง โดยจะใช้ภาษาอะไรก็ได้ แต่ในตัวอย่างจะใช้ C\# นะ แต่ไม่ต้องห่วงตอนสุดท้ายมีบอกหมดว่าภาษาอื่นๆจะทำแบบนี้ได้ยังไง

{% hint style="success" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นหนึ่งในซีรี่ AI ดังนั้นถ้าเพื่อนสนใจของสนุกๆ เช่น [**Login ด้วยใบหน้า**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/faceauth) หรือ **ยืนยันตัวตนด้วยเสียง** [**แปลงภาพเป็นข้อความ**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/ocr) และอื่นๆ สามารถดูเนื้อหาทั้งหมดได้จาก side menu ในหมวดของ **Cognitive Services** ครับ กำลังทำเรื่อยๆอยู่ ส่วนถ้าอยากรู้ว่า AI สำเร็จรูปตัวอื่นๆของ Microsoft Azure มีอะไรน่าเล่นบ้าง ไปอ่านกันได้จากลิงค์นี้เลยครัช [👶 Azure Cognitive Services](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services) เชื่อผมเต๊อะ AI ไม่ได้ยากแบบที่คิด
{% endhint %}

## 😗 ทำความเข้าใจกันก่อน

ตัวโปรแกรมที่ผมจะทำ มันจะให้เราเลือกรูปที่จะใช้ในการ Login ซึ่งถ้ารูปนั้นเป็นใบหน้าที่ถูกต้อง มันก็จะให้เรา Login ผ่าน แต่ถ้ารูปไม่ถูกก็แค่บอกว่า Login ไม่ผ่านเพียงเท่านี้ ซึ่งตัวอย่างนี้ผมจะไม่ทำ UI เลยเพราะเน้นไปที่เรื่องการใช้ AI อย่างเดียวเท่านั้นครับ

อย่างที่บอกว่าตัวอย่างนี้ผมจะใช้ภาษา C\# เขียน ดังนั้นใครที่จะทำตามด้วย C\# รบกวนลง `Visual Studio Code` และ `.NET Core SDK` ด้วย **ส่วนภาษาอื่นๆให้ดูขั้นตอนทั้งหมดให้เข้าใจก่อน** แล้วตอนสุดท้ายจะมีบอกหมดเลยว่าทำยังไงเพราะขั้นตอนทั้งหมดมันเหมือนกันต่างกันแค่วิธีเขียน

* [Download Visual Studio Code](https://code.visualstudio.com/)
* [Download .NET Core SDK](https://dotnet.microsoft.com/download)

## 🤔 เริ่มยังไงดี ?

ก่อนที่เราจะเริ่มเขียนโปรแกรมเราต้องเข้าใจก่อนว่าจะใช้งาน **Face API** นั้นเราต้องทำอะไรบ้าง ดังนั้นด้านล่างนี้คือสิ่งที่เราต้องทำทั้งหมด

1. สร้าง **Face Serivce** เพื่อจะได้ใช้ AI ได้
2. สร้าง **PersonGroup** บน Azure ขึ้นมา เจ้าตัวนี้มีหน้าที่เก็บข้อมูลคนหลายๆคนไว้
3. สร้าง **Person** เข้าไปใน PersonGroup หรือพูดง่ายๆคือสร้างคน
4. อัพโหลดรูปของหน้าเราหลายๆรูปลงไปใน Person ที่สร้างขึ้นมา
5. สั่งให้ AI เรียนรูปว่ารูปใน Person นั้นหน้าตาเป็นยังไง \(เราเรียขั้นตอนนี้ว่า **Train Model**\)
6. ทดลองเอารูปที่จะใช้ Login ไปให้ AI ดูว่าเป็นคนเดียวกับที่อยู่ใน Person นั้นหรือเปล่า

จากที่ว่ามาเราก็จะเริ่มทำทีละขั้นตอนกันเลย เพื่อทำระบบ Login ด้วยใบหน้ากัน ปะลุยๆ

## 🔥 \(1\) สร้าง Face Service กัน

{% hint style="info" %}
**Azure Portal**  
เนื้อหาตรงจุดนี้จะต้องเข้าไปที่ทำที่เว็บ [https://portal.azure.com](https://portal.azure.com) นี้นะครับ ซึ่งเราต้องสมัครสมาชิกก่อน แต่ถ้าใครยังไม่ได้สมัครก็ไปสมัครให้เรียบร้อยแซ๊ร [\(วิธีสมัครจิ้มตรงนี้\)](https://saladpuk.gitbook.io/learn/cloud/azure101/register)
{% endhint %}

1.หลังจากที่ Login เข้ามาละ ที่เมนูด้านซ้ายมือให้เลือก **`+ Create a resource`** ซะ แล้วเมนูในหน้าตรงกลางให้เลือก **`AI + Machine Learning`** แล้วจะเห็น **`Face`** ให้จิ้มมันเข้าไปเบย

![](../../.gitbook/assets/image%20%2841%29.png)

2.ถัดมาก็ใส่รายละเอียดของ Face Service ให้เรียบร้อยซะ แล้วก็กดปุ่ม **`Create`** ได้เลย

| ชื่อ | รายละเอียด |
| :--- | :--- |


| Name | ชื่อ Face Service ที่จะสร้าง |
| :--- | :--- |


<table>
  <thead>
    <tr>
      <th style="text-align:left">Location</th>
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
      <th style="text-align:left">Pricing tier</th>
      <th style="text-align:left">
        <p>&#xE08;&#xE30;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE02;&#xE32;&#xE40;&#xE01;&#xE47;&#xE1A;&#xE40;&#xE07;&#xE34;&#xE19;&#xE40;&#xE23;&#xE32;&#xE41;&#xE1A;&#xE1A;&#xE44;&#xE2B;&#xE19;</p>
        <p>&#xE16;&#xE49;&#xE32;&#xE08;&#xE30;&#xE25;&#xE2D;&#xE07;&#xE40;&#xE25;&#xE48;&#xE19;&#xE40;&#xE09;&#xE22;&#xE46;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE25;&#xE37;&#xE2D;&#xE01;
          F0 &#xE04;&#xE23;&#xE31;&#xE1A; &#xE2A;&#xE48;&#xE07;&#xE02;&#xE49;&#xE2D;&#xE21;&#xE39;&#xE25;&#xE44;&#xE14;&#xE49;
          30K &#xE04;&#xE23;&#xE31;&#xE49;&#xE07;/&#xE40;&#xE14;&#xE37;&#xE2D;&#xE19;</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

3.เรียบร้อยครับ ที่เหลือก็แค่รอให้มันสร้าง Face Service จนเสร็จ

![](../../.gitbook/assets/image%20%28405%29%20%281%29.png)

## 🔥 \(2\) สร้าง PersonGroup

ในขั้นตอนนี้ผมจะสร้างโปรเจคของ C\# ขึ้นมา เพื่อเขียนโค้ดในการสร้าง PersonGroup ล่ะนะ ดังนั้นขอแบ่งเป็นหัวข้อย่อยๆนิดหน่อย

### 2.1.สร้างโปรเจค

เริ่มต้นก็เปิด command prompt หรือ terminal ขึ้นมาเลย แล้วใช้คำสั่งด้านล่างในการสร้างโปรเจคได้เลย

```text
dotnet new console -n saladpuk-faceauth
```

> `saladpuk-faceauth` คือชื่อโปรเจคนะครับ อยากได้ชื่ออื่นก็เปลี่ยนได้เลย

ตอนนี้เราก็จะได้โปรเจคมา 1 ตัวละ ถัดไปก็เข้าไปที่โปรเจคนั้นครับด้วยคำสั่งด้านล่างนี้ \(ใครที่เปลี่ยนชื่อโปรเจคก็ใส่ชื่อเป็นชื่อโปรเจคที่ตัวเองตั้งไว้นะ\)

```text
cd saladpuk-faceauth
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

ในตัวอย่างนี้การที่เราจะเรียกใช้ Face API เราจะต้องทำงานผ่าน REST API ธรรมดานี่แหละ ดังนั้นเพื่อความง่ายผมจะลง library เสริม 2 ตัวเพื่อให้ทำงานกับ REST API และ Json ได้ง่ายขึ้นครับ โดยใช้คำสั่งด้านล่างก็เป็นอันเสร็จสิ้นพิธี

```text
dotnet add package RestSharp --version 106.6.10
dotnet add package Newtonsoft.Json
```

คราวนี้ให้เราเปิด Visual Studio Code ขึ้นมา โดยใช้คำสั่ง `code .` ภายใน command prompt/terminal ได้เลย หรือจะเปิด Visual Studio Code แล้วเปิด folder มาที่ตัวโปรเจคก็ได้ แล้วก็เปิดไฟล์ `Program.cs` ขึ้นมารอเลย

![](../../.gitbook/assets/image%20%28234%29%20%281%29.png)

{% hint style="info" %}
**Visual Studio Code: C\# Extension**  
ในตัวอย่างที่เป็นสีสวยงาม เพราะผมลง extension 2 ตัวให้กับ Visual Studio Code นะครับ ถ้าสนใจก็กดติดตั้งได้จากลิงค์ด้านล่างเลย

* [C\#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [C\# Extensions](https://marketplace.visualstudio.com/items?itemName=jchannon.csharpextensions)
{% endhint %}

### 2.2.เขียนโค้ดสร้าง PersonGroup

ตอนนี้ให้เอาโค้ดด้านล่างไปทับใน `Program.cs` ทั้งหมดเลย ซึ่งเจ้าโค้ดด้านล่างจะเป็นแค่โครงคร่าวๆเท่านั้น

{% code title="Program.cs" %}
```csharp
using System;
using System.Linq;
using System.Net;
using Newtonsoft.Json.Linq;
using RestSharp;

namespace saladpuk_faceauth
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
    }
}
```
{% endcode %}

ในโค้ดด้านบน เราจะต้องไปเอา **SubscriptionKey** และ **Endpoint** มาใส่ให้มัน เพื่อที่มันจะได้ต่อไปยัง Face API ได้นั่นเอง ซึ่งเจ้าเราต้องกลับไปที่ Cognitive Service แล้วเลือกเมนู **`Keys`** เพื่อ copy ค่า SubscriptionKey มาครับ

![](../../.gitbook/assets/image%20%28818%29.png)

ส่วนค่า Endpoint เราสามารถเอาได้จากเมนู **`Overview`** ครับตามรูปเลย

![](../../.gitbook/assets/image%20%28730%29.png)

หลังจากที่เอา `SubscriptionKey` และ `Endpoint` ไปใส่ในโค้ดแล้ว ถัดไปเราก็จะเพิ่มโค้ดอีกนิสนุง เพื่อสร้าง **PersonGroup** นั่นเอง ตามโค้ดด้านล่างเบย

{% code title="Program.cs" %}
```csharp
static string SubscriptionKey = "729c47c3f04346dd904cac8ef8181412";
static string Endpoint = "https://saladpuk-face.cognitiveservices.azure.com/face/v1.0";

static void Main(string[] args)
{
    var client = new RestClient(Endpoint);

    // สร้าง PersonGroup
    Console.WriteLine($"Create a PersonGroup.");
    var personGroupId = Guid.NewGuid().ToString().ToLower();
    var createPersonGroupReq = CreateRestRequest($"persongroups/{personGroupId}", new
    {
        name = "Prime Minister",
    });
    var createPersonGroupResult = client.Execute(createPersonGroupReq, Method.PUT);
    if (createPersonGroupResult.StatusCode == HttpStatusCode.OK)
    {
        Console.WriteLine($"-> Done. PersonGroup Id: {personGroupId}");
    }
    else
    {
        Console.WriteLine($"Error: {createPersonGroupResult.Content}");
        return;
    }

    // โค้ดขั้นตอนที่ 3 เอามาใส่ตรงนี้
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
```
{% endcode %}

**อธิบายโค้ด**  
ในการสร้าง PersonGroup เราก็แค่เรียก REST API ไปโดยการส่ง SubscriptionKey ไปยัง Endpoint ของเราเพียงเท่านี้ก็จะสามารถสร้าง PersonGroup ได้แล้วครับ

## 🔥 \(3\) สร้าง Person เข้าไปใน PersonGroup

เพื่อให้ AI เรารู้จักคนได้หลายๆคน เราก็จะสร้าง Person ให้มันครับ โดยเขียนโค้ดตัวนี้ต่อเข้าไปใน method Main\(\)

{% code title="Program.cs" %}
```csharp
// สร้าง Person
var personId = string.Empty;
Console.WriteLine($"Create a Person");
var createPersonGroupPersonRequest = CreateRestRequest($"persongroups/{personGroupId}/persons", new
{
    name = "Prayut Chan O Char",
});
var createPersonGroupPersonResult = client.Execute(createPersonGroupPersonRequest, Method.POST);
if (createPersonGroupPersonResult.StatusCode == HttpStatusCode.OK)
{
    personId = JObject.Parse(createPersonGroupPersonResult.Content)["personId"].ToString();
    Console.WriteLine($"-> Done. {createPersonGroupPersonResult.Content}");
}
else
{
    Console.WriteLine($"Error: {createPersonGroupPersonResult.Content}");
    return;
}
```
{% endcode %}

**อธิบายโค้ด**  
ผมทำการสร้าง Person ที่ชื่อ `Prayut Chan O Char` ขึ้นมาภายใต้ **PersonGroup** ที่สร้างขึ้นมาจากขั้นตอน 2.2 ครับ

## 🔥 \(4\) ทำการอัพโหลดรูปเข้าไปใน Person

ถัดมาผมก็จะทำการอัพโหลดรูปคนเข้าไปใน Person โดยรูปที่ผมอัพโหลดเข้าไปจะเอามาจากเน็ทนะครับ ตามโค้ดและรูปด้านล่างเลย

![&#xE16;&#xE49;&#xE32;&#xE44;&#xE21;&#xE48;&#xE40;&#xE2B;&#xE47;&#xE19;&#xE1C;&#xE21;&#xE40;&#xE02;&#xE35;&#xE22;&#xE19;&#xE1A;&#xE17;&#xE04;&#xE27;&#xE32;&#xE21;&#xE15;&#xE48;&#xE2D; &#xE01;&#xE47;&#xE41;&#xE2A;&#xE14;&#xE07;&#xE27;&#xE48;&#xE32;&#xE42;&#xE14;&#xE19;&#xE1B;&#xE23;&#xE31;&#xE1A;&#xE17;&#xE31;&#xE28;&#xE19;&#xE04;&#xE15;&#xE34;&#xE2D;&#xE22;&#xE39;&#xE48;&#xE19;&#xE30;](../../.gitbook/assets/image%20%2884%29.png)

{% code title="Program.cs" %}
```csharp
// อัพโหลดรูปคนที่จะใช้ในการยืนยันตัวตน
Console.WriteLine("Uploading your images.");
var personImageUrls = new[]
{
    @"https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Prayut_Chan-o-cha_%28cropped%29_2016.jpg/220px-Prayut_Chan-o-cha_%28cropped%29_2016.jpg",
    @"https://www.straitstimes.com/sites/default/files/styles/article_pictrure_780x520_/public/articles/2018/09/24/yq-prayutchan-24092018.jpg",
    @"http://www.asianews.eu/sites/default/files/public/uploads/field_s_photos/prayut_chan-o-cha.jpg",
    @"https://i.pinimg.com/originals/f1/ea/c8/f1eac8e52461ac10c61089a3ffe2c016.jpg",
    @"https://pbs.twimg.com/profile_images/1084270206938099712/qR9TdPQD_400x400.jpg",
    @"https://news.mthai.com/app/uploads/2017/08/19-08-17.jpg",
    @"https://www.newsringside.com/wp-content/uploads/2018/01/4DQpjUtzLUwmJZZCzaRpSFJqsuBh3xw4QRClWyUWGED4.jpg",
    @"https://www.sunnewsonline.com/wp-content/uploads/2018/10/Thai-PM.jpg",
    @"https://images.financialexpress.com/2017/09/download-4-14.jpg",
    @"http://www.thaiembassy.org/hanoi/contents/images/text_editor/images/PM1.jpg",
};
var uploadedCounter = 0;
foreach (var imgUrl in personImageUrls)
{
    var addFaceRequest = CreateRestRequest($"persongroups/{personGroupId}/persons/{personId}/persistedFaces", new
    {
        url = imgUrl,
    });
    var addFaceResult = client.Execute(addFaceRequest, Method.POST);
    if (addFaceResult.StatusCode == HttpStatusCode.OK)
    {
        Console.WriteLine($"-> an image has been uploaded. {++uploadedCounter}/{personImageUrls.Count()}");
    }
    else
    {
        Console.WriteLine($"Error: {addFaceResult.Content}");
    }
}
Console.WriteLine("-> All Done.");
```
{% endcode %}

**อธิบายโค้ด**  
อันนี้ผมสร้างลิสต์ของรูปขึ้นมา แล้วก็ทยอยอัพโหลดเข้าไปใน Person ทีละอันจนครบครับ

## 🔥 \(5\) สั่งให้ AI เรียนรู้ใบหน้า \(Train Model\)

หลังจากที่เราอัพโหลดรูปไปเรียบร้อยแล้ว ถัดมาเราก็จะสั่งให้ AI ไปทำการเรียนรู้รูปพวกนั้น เพื่อสร้าง pattern ใบหน้าของคนใน PersonGroup ของเรา โดยผมก็จะเพิ่มโค้ดด้านล่างนี้เข้าไปครับ

{% code title="Program.cs" %}
```csharp
// สั่งให้ AI เรียนรู้หน้าตาของ Person (Train Model)
Console.WriteLine("Training your model.");
var trainModelRequest = CreateRestRequest($"persongroups/{personGroupId}/train", null);
var trainModelResult = client.Execute(trainModelRequest, Method.POST);
if (trainModelResult.StatusCode == HttpStatusCode.Accepted)
{
    Console.WriteLine($"-> Done.");
}
else
{
    Console.WriteLine($"Error: {trainModelResult.Content}");
}
```
{% endcode %}

**อธิบายโค้ด**  
อันนี้ไม่มีอะไรเลยก็แค่เรียก REST API สั่งให้มัน Train ธรรมดา

{% hint style="success" %}
**ความแม่นยำของ AI**  
ยิ่งเราเอารูปของคนๆนั้นให้ AI เรียนรู้เยอะมากเท่าไหร่ ความแม่นยำก็จะเพิ่มขึ้นเยอะเท่านั้น ดังนั้นถ้าจะทำระบบตรวจสอบใบหน้าจริงๆเราจำเป็นต้องส่งรูปขึ้นไปเยอะพอสมควรครับ แต่สำหรับตัวอย่างเอาแค่ 5-10 รูปก็เพียงพอแล้วครับ
{% endhint %}

## 🔥 \(6\) ลองอัพโหลดรูปไปทดสอบมันจิ๊

ในรอบนี้ผมก็จะลองส่งรูปที่ไม่ใช่หน้าลุงเข้าไปดูซิว่ามันจะ บอกว่ายังไงโดยใช้รูปและโค้ดด้านล่างนี้ครับ

![&#xE25;&#xE2D;&#xE07;&#xE14;&#xE39;&#xE0B;&#xE34;&#xE27;&#xE48;&#xE32;&#xE04;&#xE19;&#xE19;&#xE35;&#xE49;&#xE08;&#xE30;&#xE41;&#xE2D;&#xE1A;&#xE21;&#xE32;&#xE2A;&#xE27;&#xE21;&#xE23;&#xE2D;&#xE22;&#xE40;&#xE1B;&#xE47;&#xE19;&#xE25;&#xE38;&#xE07;&#xE44;&#xE14;&#xE49;&#xE44;&#xE2B;&#xE21;&#xE19;&#xE30; ?](../../.gitbook/assets/image%20%28143%29.png)

{% code title="Program.cs" %}
```csharp
static void Main(string[] args)
{
    ...

    // ทดสอบว่ารูปคนอื่นสามารถยืนยันตัวตนได้หรือไม่
    var thaksinResult = IdentifyAnImage(client, personGroupId, personId, "https://www.prachachat.net/wp-content/uploads/2017/09/13814050451381405054l.jpg");
    Console.WriteLine($"The result from using Thaksin's image is: {thaksinResult}");
}

static bool IdentifyAnImage(RestClient client, string personGroupId, string personId, string imagePath)
{
    var detectFaceRequest = CreateRestRequest("detect?returnFaceId=true", new
    {
        url = imagePath,
    });
    var detectFaceResult = client.Execute(detectFaceRequest, Method.POST);
    if (detectFaceResult.StatusCode == HttpStatusCode.OK)
    {
        var faceId = JArray.Parse(detectFaceResult.Content).First["faceId"].ToString();
        var verifyRequest = CreateRestRequest("verify", new
        {
            faceId = faceId,
            personId = personId,
            personGroupId = personGroupId,
        });
        var verifyResult = client.Execute(verifyRequest, Method.POST);
        if (verifyResult.StatusCode == HttpStatusCode.OK)
        {
            var confidenceText = JObject.Parse(verifyResult.Content)["confidence"].ToString();
            var confidence = double.Parse(confidenceText);
            return confidence >= 0.75;
        }
        else
        {
            Console.WriteLine($"Error: {verifyResult.Content}");
            return false;
        }
    }
    else
    {
        Console.WriteLine($"Error: {detectFaceResult.Content}");
        return false;
    }
}
```
{% endcode %}

**อธิบายโค้ด**  
ในรอบนี้ผมก็จะส่งรูปคนอื่นเข้าไปเทียบกับลุงแล้วเช็คว่ามีความใกล้เคียงกันกี่เปอร์เซ็นต์ ซึ่งในโค้ดผมบอกว่าต้องใกล้เคียงกัน 75% ขึ้นไปนะถึงจะผ่าน

สุดท้ายผมก็ลองเพิ่มโค้ดเพื่อเอารูปลุงแกไปตรวจสอบจริงๆละ โด้ยใช้รูปที่ไม่เคยส่งให้ AI เห็นมาก่อน และใช้โค้ดตามด้านล่างนี้

![&#xE1B;&#xE4A;&#xE32;&#xE22;&#xE17;&#xE31;&#xE28;&#xE19;&#xE30;&#xE28;&#xE36;&#xE01;&#xE29;&#xE32;&#xE0B;&#xE31;&#xE01;&#xE40;&#xE14;&#xE37;&#xE2D;&#xE19;&#xE44;&#xE21;&#xE4A; ?](../../.gitbook/assets/image%20%28151%29.png)

{% code title="Program.cs" %}
```csharp
static void Main(string[] args)
{
    ...

    // ทดสอบว่ารูปที่ถูกต้องสามารถยืนยันตัวตนได้หรือไม่
    var prayutResult = IdentifyAnImage(client, personGroupId, personId, "https://www.thairath.co.th/media/NjpUs24nCQKx5e1EaLUGBbZOOxIXurh1BCeiRWkxZwq.jpg");
    Console.WriteLine($"The result from using Prayut's image is: {prayutResult}");
}
```
{% endcode %}

**อธิบายโค้ด**  
ก็ตรงตัวครับแค่เอารูปลุงไปตรวจสอบว่าเป็นคนเดียวกับรูปที่เราส่งไปให้ AI หรือเปล่า

จากทั้งหมดที่ทำมา เราก็จะทำการลอง Run กันนะครับ โดยกด `CTRL + F5` หรือจะใช้คำสั่ง `dotnet run` ก็ได้ครับ ซึ่งก็จะได้ผลลัพท์ออกมาแบบนี้

```text
Create a PersonGroup.
-> Done. PersonGroup Id: 342ca65d-0bd4-43ce-82d7-4f9ab0fc7c71
Create a Person
-> Done. {"personId":"f248ba40-dedc-44cd-ad7f-abc7e8ed2a4c"}
Uploading your images.
-> an image has been uploaded. 1/10
-> an image has been uploaded. 2/10
-> an image has been uploaded. 3/10
-> an image has been uploaded. 4/10
-> an image has been uploaded. 5/10
-> an image has been uploaded. 6/10
-> an image has been uploaded. 7/10
-> an image has been uploaded. 8/10
-> an image has been uploaded. 9/10
-> an image has been uploaded. 10/10
-> All Done.
Training your model.
-> Done.
The result from using Thaksin's image is: False
The result from using Prayut's image is: True
```

จากผลลัพท์ก็จะเห็นแล้วนะครับในบรรทัดที่ 18 ว่าเราไม่สามารถเอาคนอื่นมาแอบอ้างเป็นลุงได้  
และในบรรทัดที่ 19 เราก็สามารถเอารูปลุงที่ AI ไม่เคยเห็นมาก่อนมาทำการยืนยันตัวตนได้ว่านี่คือรูปลุงจริงๆนะได้

## 🤔 ยาวจังของโค้ดทั้งหมดหน่อย

ผมรอพิมพ์ถึงตรงนี้เหมือนกัน ง่วงนอนมากละตอนทำเรื่องนี้ แต่พอดีเนื้อหาค่อนข้างยาวดังนั้นผมขอยก source code ให้ไปดาวโหลดมาลองเล่นเลยดีกว่าครับ

{% file src="../../.gitbook/assets/saladpuk-faceauth.zip" caption="Source code: Face authentication" %}

## 🎯 บทสรุป

ในการทำงานกับ AI จริงๆไม่ใช่เรื่องยากเลยหัวใจหลักของมันจริงๆก็คือการเรียกใช้ REST API ให้ถูกตัวเท่านั้น ดังนั้นไม่ว่าเราจะเขียนภาษาไหนก็ตาม เราก็สามารถเรียกใช้ Face API เพื่อทำของประมาณนี้ได้เลย

{% hint style="success" %}
**Cognitive Services API**  
หากใครสนใจอยากดู API ทั้งหมดที่ Microsoft เตรียมไว้ให้เราเรียกใช้ AI สำเร็จรูป ก็สามารถเข้าไปดูได้จากลิงค์ด้านล่างนี้เลยครับ

* [Microsoft Cognitive Services API](https://southeastasia.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
{% endhint %}

{% hint style="success" %}
**Cognitive Services Library**  
สำหรับคนที่ต้องการเขียนทำงานกับ Cognitive Services จริงๆไม่ต้องไปนั่งเขียนเชื่อม API ทีละตัวก็ได้นะ เพราะทาง Microsoft นั้นได้มี Library ให้เราสามารถเรียกใช้ได้เลยครับ เช่นในฝั่ง .NET ก็จะมีตัวนี้ **Microsoft.Azure.CognitiveServices.Vision.Face** ที่สามารถติดตั้งแล้วใช้งานได้เลย
{% endhint %}

