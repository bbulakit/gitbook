---
description: ลองใช้ AI แยกลุงตู่กับลุงแม้วกัน ใช้ภาษาอะไรก็ทำได้ !!
---

# เขียน AI แยกของต่างๆทำยังไง?

![](../../.gitbook/assets/image%20%28517%29.png)

ในรอบนี้เราจะลองเขียนโค้ดให้โปรแกรมเราสามารถแยกแยะสิ่งของต่างๆกัน เช่น **รูปนี้คือลุงตู่** หรือ **รูปนี้คือลุงแม้ว** กันดูบ้าง ซึ่งการที่จะทำแบบนี้ได้ผมจะใช้ AI สำเร็จรูปของ Microsoft Azure ที่ชื่อว่า **Cognitive Services** ครับ

{% hint style="success" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นหนึ่งในซีรี่ AI ดังนั้นถ้าเพื่อนสนใจของสนุกๆ เช่น [**Login ด้วยใบหน้า**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/faceauth) หรือ **ยืนยันตัวตนด้วยเสียง** [**แปลงภาพเป็นข้อความ**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/ocr) และอื่นๆ สามารถดูเนื้อหาทั้งหมดได้จาก side menu ในหมวดของ **Cognitive Services** ครับ กำลังทำเรื่อยๆอยู่ ส่วนถ้าอยากรู้ว่า AI สำเร็จรูปตัวอื่นๆของ Microsoft Azure มีอะไรน่าเล่นบ้าง ไปอ่านกันได้จากลิงค์นี้เลยครัช [👶 Azure Cognitive Services](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services) เชื่อผมเต๊อะ AI ไม่ได้ยากแบบที่คิด
{% endhint %}

## 😗 ทำความเข้าใจกันก่อน

ตัวโปรแกรมของเราจะทำการส่งรูปไปให้ AI สองกลุ่มคือ **กลุ่มลุงตู่** และ **กลุ่มลุงแม้ว** โดยกลุ่มลุงตู่เราก็จะอัพโหลดรูปของลุงเข้าไปเยอะๆ ส่วนกลุ่มลุงแม้วก็อัพเฉพาะแต่รูปลุงแม้วเช่นกัน แล้วเราก็จะสั่งให้ AI ไปเรียนรู้ว่ารูปแต่ละเป็นมีลักษณะเฉพาะตัวยังไง แล้วสุดท้ายเราก็จะลองส่งรูปที่ AI ไม่เคยเห็นไปถามมันว่ารูปนี้คือรูปลุงตู่หรือลุงแม้วกันแน่ ซึ่งตัวอย่างนี้ผมจะไม่ทำ UI เลยเพราะเน้นไปที่เรื่องการใช้ AI อย่างเดียวเท่านั้นครับ

ตัวอย่างนี้ผมจะใช้ภาษา C\# เขียน ดังนั้นใครที่จะทำตามด้วย C\# ให้ลง `Visual Studio Code` และ `.NET Core SDK` ในลิงค์ด้านล่างด้วยถ้ายังไม่มี **ส่วนภาษาอื่นๆก็สามารถทำตามได้เหมือนกัน** เพราะขั้นตอนทั้งหมดเราเรียกใช้ **REST API** เพียงอย่างเดียวเลยครับ

* [Download Visual Studio Code](https://code.visualstudio.com/)
* [Download .NET Core SDK](https://dotnet.microsoft.com/download)

## 🤔 เริ่มยังไงดี ?

ก่อนที่จะเริ่มทำอะไรผมอยากให้เข้าใจตรงกันก่อนว่า ในตัวอย่างนี้เราจะต้องทำอะไรกันบ้างตามนี้เลย

1. ขั้นตอนแรกเราต้องสร้าง **Cognitive Services** ภายใน Custom Vision 2 ตัวเสียก่อน เพื่อที่จะใช้งาน AI สำเร็จรูปได้ แล้ว**เอา Key กับ Endpoint มา** ซึ่งเจ้าสองตัวนี้จะเป็นเหมือนรหัสลับในการเข้าใช้งาน AI ของเรานั่นเอง
2. สร้างโปรเจคขึ้นมาเพื่อเตรียมโค้ด
3. เขียนโค้ดอัพโหลดรูป**กลุ่มลุงตู่** กับ **กลุ่มลุงแม้ว**ให้กับ AI
4. สั่งให้ AI ทำการเรียนรู้ลักษณะเฉพาะของรูปลุงๆทั้งสอง
5. อัพโหลดรูปที่ AI ไม่เคยเห็นมาก่อน เพื่อถามมันว่ารูปนี้คือใคร

## 🔥 \(1\) สร้าง Cognitive Services กัน

1.ในขั้นตอนนี้เราจะสร้าง Cognitive Services ผ่านเว็บของ Custom Vision กัน ดังนั้น เราก็เข้าไปที่เว็บไซต์ของเขากันเลย [https://www.customvision.ai](https://www.customvision.ai/) พอเข้าไปแล้วก็จะเห็นปุ่ม **`SIGN IN`** ตัวใหญ่ๆก็ทำการ login เข้าไปเลยครับ

![](../../.gitbook/assets/image%20%28111%29.png)

2.เมื่อเข้ามาแล้วให้กดรูปฟังเฟืองด้านบนขวาครับ

![](../../.gitbook/assets/image%20%28134%29.png)

3.กดปุ่ม `Create new` เพื่อทำการสร้าง Cognitive Service ตัวแรกกัน ซึ่งตัวนี้เราจะสร้างเป็นตัวที่ใช้สำหรับ `Train Model` หรือพูดง่ายๆว่าเป็นตัวที่สามารถทำให้ AI เก่งขึ้นไปเรื่อยๆได้

| ชื่อ | รายละเอียด |
| :--- | :--- |


| **Name** | ชื่อ Cognitive Services ที่จะสร้าง |
| :--- | :--- |


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

| **King** | ประเภทของของการใช้งาน ตัวแรกนี้ให้เลือกเป็น **`CustomVision.Training`** ครับ |
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
        <p>&#xE08;&#xE30;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE02;&#xE32;&#xE40;&#xE01;&#xE47;&#xE1A;&#xE40;&#xE07;&#xE34;&#xE19;&#xE40;&#xE23;&#xE32;&#xE41;&#xE1A;&#xE1A;&#xE44;&#xE2B;&#xE19;</p>
        <p>&#xE16;&#xE49;&#xE32;&#xE2D;&#xE22;&#xE32;&#xE01;&#xE43;&#xE0A;&#xE49;&#xE15;&#xE31;&#xE27;&#xE1F;&#xE23;&#xE35;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE25;&#xE37;&#xE2D;&#xE01;
          F0 &#xE04;&#xE23;&#xE31;&#xE1A;</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

เรียบร้อยแล้วเราก็จะได้ Service ที่ใช้สำหรับ **`Training`** ออกมา 1 ตัวละ คราวนี้ให้จด **`Key`** กับ **`Endpoint`** ไว้ครับ เดี๋ยวเราจะเอาไปใช้ในขั้นตอนถัดไปต่อ

![](../../.gitbook/assets/image%20%28619%29.png)

4.ให้กด `create new` เพื่อทำการสร้างอีก 1 ตัวซึ่งตัวที่สองนี้เราจะใช้เป็นตัว `Prediction` ครับ หรือพูดง่ายๆว่า ใช้เป็นตัวสำหรับเอาไว้ให้คนอื่นๆเรียกใช้งานได้นั่นเอง ซึ่งเจ้าตัวนี้จะไม่ให้มันสามารถมาสั่ง train model เราได้

![](../../.gitbook/assets/image%20%28610%29.png)

เรียบร้อยแล้วเราก็จะได้ Service ที่ใช้สำหรับ **`Prediction`** ออกมา 1 ตัวละ คราวนี้ให้จด **`Key`** กับ **`Endpoint`** ไว้ครับ เดี๋ยวเราจะเอาไปใช้ในขั้นตอนถัดไปต่อ

{% hint style="danger" %}
**คำเตือน**  
ตัว Service ที่สร้าง ถ้าเลือก Pricing Tier เป็น F0 จะไม่เสียเงินก็จริง แต่มีข้อจำกัดคือสร้าง project มาเล่นได้ไม่เกิน 2 ตัว และจำกัดจำนวนครั้งในการสร้าง tag บลาๆ ดังนั้นถ้าใครลอง Run โค้ดแล้วไม่ผ่านอาจเป็นเพราะโค้ดไปสร้าง project จนมันครบกำหนดแล้วก็ได้ ซึ่งวิธีการลบโปรเจคจะอยู่หัวข้อสุดท้ายเลยครับ
{% endhint %}

## 🔥 \(2\) สร้างโปรเจค

ในขั้นตอนนี้ผมจะสร้างโปรเจคของ C\# ขึ้นมา เพื่อเขียนโค้ดในการสร้างเรียกใช้ Cognitive Services ล่ะนะ ดังนั้นขอแบ่งเป็นหัวข้อย่อยๆนิดหน่อย

### 2.1.สร้างโปรเจค

เริ่มต้นก็เปิด command prompt หรือ terminal ขึ้นมาเลย แล้วใช้คำสั่งด้านล่างในการสร้างโปรเจคได้เลย

```text
dotnet new console -n saladpuk-image-classification
```

> `saladpuk-image-classification` คือชื่อโปรเจคนะครับ อยากได้ชื่ออื่นก็เปลี่ยนได้เลย

ตอนนี้เราก็จะได้โปรเจคมา 1 ตัวละ ถัดไปก็เข้าไปที่โปรเจคนั้นครับด้วยคำสั่งด้านล่างนี้ \(ใครที่เปลี่ยนชื่อโปรเจคก็ใส่ชื่อเป็นชื่อโปรเจคที่ตัวเองตั้งไว้นะ\)

```text
cd saladpuk-image-classification
```

ในตัวอย่างนี้การที่เราจะเรียกใช้ Cognitive Service โดยใช้ Library ที่ Microsoft เขาเตรียมมาให้เราดูบ้างละนะ ดังนั้นเราก็ต้องจะลง library เสริม 2 โดยใช้คำสั่งด้านล่างก็เป็นอันเสร็จสิ้นพิธี

```text
dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training
dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction
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
using System.Threading;
using Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction;
using Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training;
using Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training.Models;

namespace saladpuk_image_classification
{
    class Program
    {
        const string Endpoint = "ENTER_YOUR_ENDPOINT";
        const string TrainingKey = "ENTER_YOUR_TRAINING_KEY";
        const string PredictionKey = "ENTER_YOUR_PREDICTION_KEY";
        const string PredictionResourceId = "ENTER_YOUR_PREDICTION_RESOURCE_ID";

        static void Main(string[] args)
        {
            // เอาโค้ดจากขั้นตอนที่ 3 มาใส่ตรงนี้เบย
        }
    }
}
```
{% endcode %}

จากโค้ดด้านบนเราจะต้องเอา `Key` กับ `Endpoint` ที่ได้มาจากขั้นตอนที่ 1 เอามาใส่ไว้ในโค้ดของเราในบรรทัดที่ 14-17 เพื่อเตรียมให้เราสามารถเรียก Cognitive Services ได้นั่นเอง ดังนั้นก็ไปเอามาใส่กันเบย

| ชื่อ | เอามาจากไหน |
| :--- | :--- |
| Endpoint | ขั้นตอนที่ 1 ที่กดจัดฟันเฟือง |
| TrainingKey | **`Key`** ของ Service ที่เราสร้างเป็น Training |
| PredictionKey | **`Key`** ของ Service ที่เราสร้างเป็น Prediction |
| PredictionResourceId | **`Resource Id`** ของ Service ที่เราสร้างเป็น Prediction |

{% code title="Program.cs" %}
```csharp
const string Endpoint = "https://southeastasia.api.cognitive.microsoft.com/";
const string TrainingKey = "9307b0e4690f43219a136680e5b59740";
const string PredictionKey = "f8b0f6ce2cda4b7d8cef892b3f3de468";
const string PredictionResourceId = "/subscriptions/c91771fb-def2-45f8-9400-673d59a95640/resourceGroups/saladpuk-demo/providers/Microsoft.CognitiveServices/accounts/saladpuk-prediction";
```
{% endcode %}

{% hint style="danger" %}
**คำเตือน**  
ถ้าใช้ **Key** กับ **Endpoint** จากตัวอย่างของผมมันจะทำงานไม่ได้นะครับ เพราะผมลบของพวกนี้ไปแล้ว ให้ไปสร้างมาเล่นเองนะ
{% endhint %}

## 🔥 \(3\) เขียนโค้ดอัพโหลดรูปลุงทั้งหาย

ในรอบนี้เราก็จะเพิ่มโค้ดเพื่อที่จะทำการอัพโหลดรูปลุงตู่หลายๆรูปขึ้นไป โดยรูปที่ใช้ก็จะราวๆด้านล่างนี้เลย

![&#xE23;&#xE39;&#xE1B;&#xE01;&#xE25;&#xE38;&#xE48;&#xE21;&#xE25;&#xE38;&#xE07;&#xE15;&#xE39;&#xE48;](../../.gitbook/assets/image%20%2884%29.png)

![&#xE23;&#xE39;&#xE1B;&#xE01;&#xE25;&#xE38;&#xE48;&#xE21;&#xE25;&#xE38;&#xE07;&#xE41;&#xE21;&#xE49;&#xE27;](../../.gitbook/assets/image%20%28212%29.png)

เพื่อความง่ายในการหารูปเพื่อนๆสามารถดาวโหลดรูปได้จากลิงค์ด้านล่างนี้เลยนะครับ พอโหลดเสร็จให้แตกไฟล์ zip แล้วจะเจอโฟเดอร์ **`images`** ให้เอาเจ้าโฟเดอร์นี้ไปใส่ไว้ใน project ที่สร้างไว้จากขั้นตอนที่ 2 เลยครัช

{% file src="../../.gitbook/assets/images.zip" caption="รูปลุงๆทั้งหลาย" %}

หลังจากที่เอาโฟเดอร์มาใส่ใน project แล้วถัดไปเราก็จะเริ่มเขียนโค้ดเพื่ออัพโหลดรูปลุงๆกันต่อนะครับ โดยโค้ดตัวแรกคือสั่งให้ไปสร้าง project ใน Custom Vision โดยใช้โค้ดด้านล่างนี้ครัช

{% code title="Program.cs" %}
```csharp
var trainingClient = new CustomVisionTrainingClient()
{
    ApiKey = TrainingKey,
    Endpoint = Endpoint
};

Console.WriteLine("Creating new project:");
var project = trainingClient.CreateProject("Saladpuk demo");
```
{% endcode %}

หลังจากที่สร้าง project แล้ว ถัดไปเราก็จะอัพโหลดรูปลุงๆ โดยใช้โค้ดตัวถัดมา

อัพโหลดรูปลุงตู่ก่อน

{% code title="Program.cs" %}
```csharp
Console.WriteLine("Uploading Prayut images.");
var prayutTag = trainingClient.CreateTag(project.Id, "Prayut");
var prayutImages = Directory.GetFiles(Path.Combine("Images", "Prayut"));
var prayutFiles = prayutImages.Select(img => new ImageFileCreateEntry(Path.GetFileName(img), File.ReadAllBytes(img))).ToList();
trainingClient.CreateImagesFromFiles(project.Id, new ImageFileCreateBatch(prayutFiles, new List<Guid>() { prayutTag.Id }));
Console.WriteLine("-> Done.");
```
{% endcode %}

ถัดมาก็อัพโหลดรูปลุงแม้ว

{% code title="Program.cs" %}
```csharp
Console.WriteLine("Uploading Thaksin images.");
var thaksinTag = trainingClient.CreateTag(project.Id, "Thaksin");
var thaksinImages = Directory.GetFiles(Path.Combine("Images", "Thaksin"));
var thaksinFiles = thaksinImages.Select(img => new ImageFileCreateEntry(Path.GetFileName(img), File.ReadAllBytes(img))).ToList();
trainingClient.CreateImagesFromFiles(project.Id, new ImageFileCreateBatch(thaksinFiles, new List<Guid>() { thaksinTag.Id }));
Console.WriteLine("-> Done.");
```
{% endcode %}

## 🔥 \(4\) สั่งให้ AI รู้จักหน้าของคุณลุงไว้

หลังจากอัพโหลดทุกอย่างเสร็จแล้ว เราก็จะสั่งให้ AI มันทำการเรียนรู้รูปภาพในแต่ละกลุ่ม เพื่อให้ AI สามารถวิเคราะห์ได้ว่ากลุ่มแต่ละกลุ่มมันมีลักษณะเฉพาะตัวยังไงนั่นเอง ดังนั้นเลยเอาโค้ดตัวนี้ไปแปะต่อเลย

{% code title="Program.cs" %}
```csharp
Console.WriteLine("Training.");
var iteration = trainingClient.TrainProject(project.Id);
while (iteration.Status == "Training")
{
    Thread.Sleep(1000);
    iteration = trainingClient.GetIteration(project.Id, iteration.Id);
}
Console.WriteLine("-> Done.");

Console.WriteLine("Publishing.");
var publishedName = "GuessWho";
trainingClient.PublishIteration(project.Id, iteration.Id, publishedName, PredictionResourceId);
Console.WriteLine("-> Done.");
```
{% endcode %}

## 🔥 \(5\) ให้ AI ทายซิว่านี่รูปใครเอ่ย ?

ถัดมาเราก็จะลองเอารูปที่ AI ไม่เคยเห็นมาก่อน ส่งไปถามเจ้า AI ว่ารูปนี้ใครเอ่ย ซึ่งรูปที่ผมจะสั่งไปก็คือรูปด้านล่างนี้

![&#xE1B;&#xE32;&#xE22;&#xE17;&#xE31;&#xE28;&#xE19;&#xE30;&#xE28;&#xE36;&#xE01;&#xE29;&#xE32;&#xE40;&#xE25;&#xE48;&#xE19;&#xE0B;&#xE31;&#xE01;&#xE40;&#xE14;&#xE37;&#xE2D;&#xE19;&#xE21;&#xE38;&#xE4A;&#xE22; ?](../../.gitbook/assets/image%20%28151%29.png)

ซึ่งรูปที่จะส่งไปก็อยู่ใน zip ไฟล์ที่ดาวโหลดไปนั่นแหละ `Images\Test\Test.jpg` ดังนั้นเราก็จะเขียนโค้ดตัวนี้ต่อ

{% code title="Program.cs" %}
```csharp
Console.WriteLine("Making a prediction.");
var predictionClient = new CustomVisionPredictionClient()
{
    ApiKey = PredictionKey,
    Endpoint = Endpoint
};
var testImage = new MemoryStream(File.ReadAllBytes(Path.Combine("Images", @"Test\Test.jpg")));
var result = predictionClient.ClassifyImage(project.Id, publishedName, testImage);
foreach (var prediction in result.Predictions)
{
    Console.WriteLine($"-> {prediction.TagName}: {prediction.Probability:P1}");
}
Console.WriteLine("-> Done.");
```
{% endcode %}

อะเช เพียงเท่านี้เราก็ลอง Run ได้เลย ซึ่งถ้าอยู่ใน Visual Studio Code สามารถกด **`CTRL + F5`** ได้เลย แต่ถ้าอยู่ใน Command prompt หรือ Terminal ก็สามารถใช้คำสั่ง **`dotnet run`** ได้เลยเหมือนกันครับ ซึ่งผลลัพท์ที่ได้ก็จะออกมาเป็นราวๆนี้

```text
Creating new project:
Uploading Prayut images.
-> Done.
Uploading Prayut images.
-> Done.
Training.
-> Done.
Publishing.
-> Done.
Making a prediction:
-> Prayut: 87.4%
-> Thaksin: 2.7%
```

จะเห็นว่ารูปที่ส่งไปทายนั้น เจ้า AI บอกว่าใกล้เคียงกับลุงตู่ 87.4% ครับป๋ม

ตัวโปรเจคนี้ถ้าใครขี้เกียจไปนั่งลบมันทุกครั้ง ก็สามารถใส่โค้ดนี้เข้าไปได้นะครับ พอได้ผลลัพท์เสร็จมันจะลบโปรเจคทิ้งให้เลย

{% code title="Program.cs" %}
```csharp
Console.WriteLine("Deleting your project.");
trainingClient.UnpublishIteration(project.Id, iteration.Id);
trainingClient.DeleteIteration(project.Id, iteration.Id);
trainingClient.DeleteProject(project.Id);
Console.WriteLine("-> Done.");
```
{% endcode %}

## 🤔 อยากเอา AI ตัวนี้ไปให้คนอื่นเล่นทำไง ?

สำหรับใครที่สร้างโปรเจคขึ้นมาแล้วอยากเอาไปให้เพื่อนๆเล่น หรือเอาไปใช้งานจริง สามารทำได้ง่ายๆโดยเรียกผ่าน REST API โดยกดเลือกโปรเจคที่ต้องการ

![](../../.gitbook/assets/image%20%28373%29.png)

แล้วที่เมนูด้านบนให้เลือก **`Performance`** แล้วเมนูด้านล่างเลือก **`Prediction URL`** ตามรูปเบย

![](../../.gitbook/assets/image%20%287%29.png)

ภายใน popup ก็จะมีรายละเอียดของการเรียกใช้งาน ดังนั้นเวลาที่เราเรียกใช้ REST API ก็ส่ง `header` และ `request body` ไปตามที่อยู่ใน popup ก็จะสามารถเรียกใช้ AI ตัวนี้ได้เลยครัช

![](../../.gitbook/assets/image%20%28885%29.png)

## 🤔 อยากลบโปรเจคทำไง ?

อย่างที่บอกไปว่าถ้าเลือกตัวฟรีเราจะสร้างโปรเจคได้ไม่เกิน 2 ตัวและมีข้อจำกัดอื่นๆด้วย ดังนั้นวิธีการลบโปรเจคคือการเข้าไปที่ตัวเว็บ Custom Vision ตามลิงค์นี้ [https://www.customvision.ai](https://www.customvision.ai/) แล้วเอาเมาส์ไปแถวๆโปรเจคที่ต้องการจะลบ แล้วกดรูปถังขยะซะก็เสร็จแล้ว

![](../../.gitbook/assets/image%20%28475%29.png)

ถ้ากดจากถังขยะแล้วไม่ได้ ให้กดโปรเจคที่ต้องการจะลบตามรูป

![&#xE40;&#xE25;&#xE37;&#xE2D;&#xE01;&#xE42;&#xE1B;&#xE23;&#xE40;&#xE08;&#xE04;&#xE17;&#xE35;&#xE48;&#xE15;&#xE49;&#xE2D;&#xE07;&#xE01;&#xE32;&#xE23;&#xE08;&#xE30;&#xE25;&#xE1A; &#xE44;&#xE21;&#xE48;&#xE43;&#xE0A;&#xE48;&#xE08;&#xE30;&#xE17;&#xE33;&#xE2D;&#xE22;&#xE48;&#xE32;&#xE07;&#xE2D;&#xE37;&#xE48;&#xE19;&#xE08;&#xE23;&#xE34;&#xE07;&#xE46;&#xE19;&#xE30;](../../.gitbook/assets/image%20%28373%29.png)

ถัดไปที่ด้านบนขวาให้เลือกไปที่เมนู **`Performance`** แล้วเมนูด้านล่างเลือก **`Unpublish`** แล้วตามด้วย **`Delete`** เสร็จแล้วก็จะสามารถกลับไปกดลบที่ถังขยะได้แล้วครัช

![](../../.gitbook/assets/image%20%28289%29.png)

![](../../.gitbook/assets/image%20%28475%29.png)

## 🤔 ยาวจังของโค้ดทั้งหมดหน่อย

พอดีเนื้อหาค่อนข้างยาวดังนั้นผมขอยก source code ให้ไปดาวโหลดมาลองเล่นเลยดีกว่าครับ

{% file src="../../.gitbook/assets/saladpuk-image-classification.zip" caption="Source Code: Image classification" %}

## 🎯 บทสรุป

ในการทำงานกับ AI จริงๆไม่ใช่เรื่องยากเลยหัวใจหลักของมันจริงๆก็คือการเรียกใช้ REST API ให้ถูกตัวเท่านั้น ดังนั้นไม่ว่าเราจะเขียนภาษาไหนก็ตาม เราก็สามารถเรียกใช้ Cognitive Services API เพื่อทำของประมาณนี้ได้เลย

ดังนั้นภาษาอื่นๆก็แค่เรียกไปที่ API ของ Custom Vision ตามลิงค์นี้ ก็จะสามารถทำงานได้เลยครัช  
[Microsoft Cognitive Services API - Custom Vision](https://southeastasia.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b7fa)

{% hint style="success" %}
**Cognitive Services API**  
หากใครสนใจอยากดู API ทั้งหมดที่ Microsoft เตรียมไว้ให้เราเรียกใช้ AI สำเร็จรูป ก็สามารถเข้าไปดูได้จากลิงค์ด้านล่างนี้เลยครับ

* [Microsoft Cognitive Services API](https://southeastasia.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
{% endhint %}

![&#xE25;&#xE32;&#xE01;&#xE48;&#xE2D;&#xE19;&#xE04;&#xE23;&#xE31;&#xE1A;&#xE17;&#xE38;&#xE01;&#xE04;&#xE19; &#xE2D;&#xE22;&#xE48;&#xE32;&#xE25;&#xE37;&#xE21;&#xE44;&#xE1B;&#xE40;&#xE22;&#xE35;&#xE48;&#xE22;&#xE21;&#xE1C;&#xE21;&#xE14;&#xE49;&#xE27;&#xE22; &#xE2D;&#xE48;&#xE2D;&#xE1C;&#xE21;&#xE0A;&#xE2D;&#xE1A;&#xE01;&#xE34;&#xE19;&#xE02;&#xE49;&#xE32;&#xE27;&#xE1C;&#xE31;&#xE14;&#xE1B;&#xE39;&#xE19;&#xE30;](../../.gitbook/assets/image%20%28399%29.png)

