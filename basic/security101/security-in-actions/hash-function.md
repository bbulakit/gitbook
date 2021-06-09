---
description: "\U0001F914 จะเปลี่ยนของธรรมดาๆให้เป็นความลับได้ยังงุย?"
---

# Hash function

อะไรก็ตามแต่ที่เกี่ยวกับการทำ Computer Security นั้น เรามักจะเปลี่ยนของธรรมดาให้เป็นความลับเสมอ เพื่อป้องกันไม่ให้คนร้ายแอบเอาไปใช้ได้ แต่คำถามแรกของเราคือ เราจะเปลี่ยนของพวกนั้นให้เป็นความลับได้ยังไงดีหว่า? 🤔

{% hint style="info" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นส่วนหนึ่งของคอร์ส [👦 Security พื้นฐาน](https://saladpuk.gitbook.io/learn/basic/security101) ถ้าเพื่อนๆสนใจอยากรู้หลักในการรักษาความปลอดภัยตั้งแต่เริ่มต้นเลย ก็กดอ่านที่ตัวสีฟ้าๆได้เลยนะ
{% endhint %}

## 😥 ปัญหา

**ดช.แมวน้ำ** เชื่อว่าเราทุกคนมีฟามลับที่ไม่อยากให้คนอื่นรู้กันหมดนั่นแหละ เช่น รหัสบัตรเครดิต/เดบิต รหัสผ่านอีเมล์ รหัสปลดล็อคมือถือ หรือพูดง่ายๆคือ **รหัสผ่านต่างๆ** นั่นเอง ซึ่งปรกตินั้นจะมีแค่เราเท่านั้นที่รู้รหัสลับพวกนั้นชิมิ? แต่ถ้าเราคิดดูดีๆก็จะพบว่า **ยังมีอีกคนที่รู้รหัสลับของเราด้วย** นั่นก็คือผู้ให้บริการของเรายังไงล่ะ เช่น เหล่าธนาคารเจ้าของบัตรเครดิต/เดบิต ผู้ให้บริการอีเมล์ต่างๆ หรือตัวมือถือเองที่รู้รหัสปลดล็อคจอมือถือ \(ถ้าผู้ให้บริการไม่รู้รหัสลับของเราแล้วเขาจะรู้ได้ไงว่าเราใส่รหัสลับถูก 🤣\)

จากที่ว่ามานั่นหมายความว่า สมมุติว่าเราเก็บความลับได้ดีฝุดๆ แต่ผู้ให้บริการโดน hack ระบบเสียเอง ความลับของเราก็จะไม่เป็นความลับอีกต่อไปอ่ะจิ ซึ่งเราในฐานะ Programmer ซึ่งมีหน้าที่เขียนโค้ดเพื่อป้องกันเรื่องเหล่านี้ จะรับมือกับการที่ถูก hack ได้ยังไงดีนะ? ... หรือต่อให้ไม่ถูก hack ก็ได้ แค่เราหรือเพื่อนร่วมงานเข้าไปอ่านข้อมูลใน database แล้วเห็น password ผู้ใช้ในระบบได้ ก็ถือว่าระบบเราตกเรื่อง Security เรียบร้อยแบ้ว!!

🤠 เรื่องนี้มีวิธีการป้องกันเยอะม๊วก ไม่ว่าจะเป็นระดับ Hardware, Software, Peopleware, Network บลาๆ แต่**สิ่งที่เป็นขั้นต่ำที่สุดและมีประสิทธิภาพที่สุดคือจุดที่เราเขียนโค้ดนี่แหละ** เพราะการป้องกันระดับอื่นๆเป็นการกันแค่เปลือกเท่านั้น สุดท้ายถ้าคนร้าย หรือ เรานี่แหละเป็นคนร้าย สามารถเข้าถึงความลับได้ก็จบกันถ้าเราไม่ได้ป้องกันเอาไว้ \(แล้วจะป้องกันยังไงให้แม้แต่คนเขียนเองก็โกงระบบตัวเองไม่ได้หว่า?\)

{% hint style="info" %}
ถ้าอยากเห็นภาพมากขึ้นลองไปอ่าน [**🦉 นิทานเรื่องที่สอง**](https://www.saladpuk.com/basic/security101/secure-password) ที่ป๋มได้เขียนไว้ได้นะ
{% endhint %}

## 😉 วิธีการแก้ปัญหา

เราก็แค่ทำให้ **ความลับไม่สามารถเอาไปใช้งานได้ตรงๆ** ก็เพียงพอแล้ว เช่น สมมุติว่าเราเก็บ Username & Password ของคนในระบบเป็นแบบด้านล่าง เมื่อคนร้ายเข้าถึงข้อมูลนี้ได้ เขาก็จะรู้เลยว่าทุกคนใช้รหัสผ่านอะไรชิมิ

| Id | Username | Password |
| :--- | :--- | :--- |
| 1 | saladpuk | 1234 |
| 2 | thaksin | homesick |
| 3 | prayut | 1234 |

ซึ่งเมื่อเรานำหลัก Security เข้ามาใช้ เราก็จะเปลี่ยนให้ความลับไม่สามารถเอาไปใช้งานได้ตรงๆ ก็จะทำให้ข้อมูลเราเปลี่ยนเป็นราวๆนี้

| Id | Username | Password |
| :--- | :--- | :--- |
| 1 | saladpuk | fd4438f5b856bb685004b84221ba794b |
| 2 | thaksin | f679b30fde31dff10304dc357300c52b |
| 3 | prayut | 3d54cf99f817a21cd9fd2b7218e58fec |

จะเห็นว่าตารางอันใหม่ด้านบนนั้น **เราได้แปลงรหัสผ่านทุกคนเป็นอย่างอื่นไปเรียบร้อยแล้ว** ซึ่งผู้ใช้ทุกคนก็ยังคงสามารถใช้รหัสผ่านอันเดิมเข้าสู่ระบบได้เหมือนเดิม แถมคนที่สร้างระบบก็ไม่สามารถรู้ได้ว่ารหัสผ่านแต่ละคนคืออะไร และจะเห็นว่าผู้ใช้ saladpuk และ prayut ที่ใช้รหัสผ่านเดียวกัน พอดูในตารางใหม่เราก็ไม่สามารถเดาได้เลยว่าเขาใช้รหัสผ่านเดียวกันด้วย

## 😅 อยากทำเป็นอ่ะ

การที่เราจะทำเรื่องนี้ได้แบบถูกหลักจะทำให้ระบบเรามีความปลอดภัยสูงขึ้น ซึ่งเราต้องอาศัยความรู้พื้นฐานที่เรียกว่าการทำ [Hash ](https://en.wikipedia.org/wiki/Hash_function)กับ [HMAC ](https://en.wikipedia.org/wiki/HMAC)และถ้าจะให้ดีควรรู้เรื่องการทำ [Salt ](https://en.wikipedia.org/wiki/Salt_%28cryptography%29)กับ [Pepper ](https://en.wikipedia.org/wiki/Pepper_%28cryptography%29)ด้วย ซึ่งกระป๋มได้เล่านิธานเพื่อให้เข้าใจง่ายๆแล้วในบทความ [🦉 นิทานเรื่องแรก](https://www.saladpuk.com/basic/security101) กับ [🦉 นิทานเรื่องที่สอง](https://www.saladpuk.com/basic/security101/secure-password) ถ้าสนใจก็ไปอ่านต่อได้กั๊ฟ 

\(ในบทความนี้จะขอสอนแค่ Hash ตัวเดียวก่อนนะ ส่วนตัวอื่นๆจะเป็นบทความถัดไป\)

## 😎 Hash Functions

การที่เราจะ **เปลี่ยนข้อความธรรมดาเป็นข้อความลับ** นั้นมีหลายวิธี ซึ่งแต่ละวิธีก็มีหลาย Algorithms ให้เราเลือกใช้เลย โดยในรอบนี้ตัวพื้นฐานที่เราจะมามาเป็นพระเอกของเราคือเจ้าตัวที่ชื่อว่า **Hash** นั่นเองกั๊ฟ ซึ่งรายละเอียดของมันรบกวนเพื่อนไปอ่านต่อที่บทความนี้นะ [**💔 จดหมายถูกแก้ไข**](https://www.saladpuk.com/basic/security101#undefined-3) ตรงนี้แมวน้ำขอสรุปแบบย่อๆไว้ละกันว่า

* การทำ Hash เป็นการแปลงข้อความธรรมดาให้เป็นข้อความอื่นที่มีความยาวเท่ากันเสมอ
* ผลลัพท์ที่ได้ไม่สามารถคำนวณย้อนกลับเพื่อไปหาว่าข้อความจริงๆคืออะไรได้
* ข้อความตั้งต้นเหมือนกันจะได้ผลลัพท์เดียวกันเสมอ
* ข้อความตั้งต้นต่างกันบางทีก็อาจจะได้ผลลัพท์เดียวกัน

เมื่อเข้าใจภาพรวมคร่าวๆของ Hash functions แล้ว ทีนี้ก็อย่างที่บอกว่ามันมีให้เราเลือกใช้หลายตัวเลย เช่น MD5, SHA-1, SHA-256, SHA-384, SHA-512 บลาๆ

{% hint style="info" %}
**เกร็ดความรู้**  
พวก Security ในสมัยก่อนนั้นเกิดมาตอนที่คอมพิวเตอร์ไม่ได้เร็วมากเท่าปัจจุบัน แต่หลังๆมานี้มือถือที่เราใช้อยู่ก็มีความเร็วมากกว่า Super Computer ในอดีตแบบขาดลอยแล้ว เลยทำให้ Security บางตัวถือว่าไม่ปลอดภัยแล้วในปัจจุบัน เช่น MD5 เป็นต้น ดังนั้นเราควรอัพเดทเรื่อง Security บ่อยๆว่าปัจจุบันเขาแนะนำตัวไหนเป็นขั้นต่ำสุดนั่นเองกั๊ฟ เพราะปัจจุบันคอมพิวเตอร์เป็นไปตามกฎ [Moore's law](https://en.wikipedia.org/wiki/Moore%27s_law) อยู่
{% endhint %}

## 🤔 โค้ดเขียนยังไง?

ตามที่เคยบอกไปว่า แต่ละภาษาจะมี library เฉพาะทางของมันเอง เช่นในฝั่งของ .NET ก็จะอยู่ใน [`System.Security.Cryptography`](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography?view=net-5.0) ซึ่งเรามีให้เลือกใช้คือ [SHA1](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.sha1?view=net-5.0), [SHA1CryptoServiceProvider](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.sha1cryptoserviceprovider?view=net-5.0), [SHA1Manged](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.sha1managed?view=net-5.0), [SHA256](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.sha256?view=net-5.0), [SHA256CryptoServiceProvider](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.sha256cryptoserviceprovider?view=net-5.0), ... \(ไปดูเองเยอะม๊วก 😭 \) โดยในรอบนี้แมวน้ำจะลองเอาข้อความ 3 ข้อความที่ไม่เหมือนกัน ไปเข้ารหัสผ่าน MD5, SHA1, SHA256 และ SHA512 ดูว่าผลลัพท์มันจะเป็นยังไงกันบ้างนะ ตามโค้ดด้านล่างนี้เบย

ข้อความ 3 ตัวที่ไม่เหมือนกัน \(ตัวเล็กตัวใหญ่ถือว่าต่างกันนะจ๊ะ\)

```csharp
var plainText1 = "Hello world";
var plainText2 = "hello world";
var longestText = "This is the longest text ever";
```

สร้าง Method ที่ใช้ในการเข้ารหัส Hash

```csharp
static string GetHashedText(string text, HashAlgorithm algorithm)
{
	var byteText = Encoding.UTF8.GetBytes(text);
	var byteResult = algorithm.ComputeHash(byteText);
	return string.Join(string.Empty, byteResult.Select(it => it.ToString("X2")));
}
```

ทดลองเข้ารหัสข้อความทั้ง 3 ผ่าน Hash แต่ละแบบ

```csharp
Console.WriteLine("MD5");
using var md5 = MD5.Create();
Console.WriteLine(GetHashedText(plainText1, md5));
Console.WriteLine(GetHashedText(plainText2, md5));
Console.WriteLine(GetHashedText(longestText, md5));

Console.WriteLine("\nSHA-1");
using var sha1 = SHA1.Create();
Console.WriteLine(GetHashedText(plainText1, sha1));
Console.WriteLine(GetHashedText(plainText2, sha1));
Console.WriteLine(GetHashedText(longestText, sha1));

Console.WriteLine("\nSHA-256");
using var sha256 = SHA256.Create();
Console.WriteLine(GetHashedText(plainText1, sha256));
Console.WriteLine(GetHashedText(plainText2, sha256));
Console.WriteLine(GetHashedText(longestText, sha256));

Console.WriteLine("\nSHA-512");
using var sha512 = SHA512.Create();
Console.WriteLine(GetHashedText(plainText1, sha512));
Console.WriteLine(GetHashedText(plainText2, sha512));
Console.WriteLine(GetHashedText(longestText, sha512));
```

ผลลัพท์

```text
MD5
3E25960A79DBC69B674CD4EC67A72C62
5EB63BBBE01EEED093CB22BB8F5ACDC3
13988F9767E8BADB3E8D98F3A7FD83F6
3E25960A79DBC69B674CD4EC67A72C62

SHA-1
7B502C3A1F48C8609AE212CDFB639DEE39673F5E
2AAE6C35C94FCFB415DBE95F408B9CE91EE846ED
F44FDE2EB97F456F4F7A11B2D96FB6923900E406

SHA-256
64EC88CA00B268E5BA1A35678A1B5316D212F4F366B2477232534A8AECA37F3C
B94D27B9934D3E08A52E52D7DA7DABFAC484EFE37A5380EE9088F7ACE2EFCDE9
756A8392F0F9F4103E9909A508EC8722FB061D81512C59060BEBE6145E57BE0A

SHA-512
B7F783BAED8297F0DB917462184FF4F08E69C2D5E5F79A942600F9725F58CE1F29C18139BF80B06C0FFF2BDD34738452ECF40C488C22A7E3D80CDF6F9C1C0D47
309ECC489C12D6EB4CC40F50C902F2B4D0ED77EE511A7C7A9BCD3CA86D4CD86F989DD35BC5FF499670DA34255B45B0CFD830E81F605DCF7DC5542E93AE9CD76F
ABA075B7537FD45343B98619C9EE5B116F9D2C43348033C249EE6DEFBB8F85DE854D7B6A63CF4365830A2A6F4EA4DF496163701FCC54D4EA41D54C83826E501F
```

จะเห็นว่าไม่ว่าจะ run กี่ครั้ง เราก็จะได้ผลลัพท์เดิมเสมอ และทุกอย่างก็จะเป็นตามกฎทั้ง 4 ข้อตามที่เคยบอกไว้ และความต่างๆของการเข้า Hash แต่ละแบบก็จะเห็นว่า ยิ่งเลขเยอะมันก็จะได้ผลลัพท์ที่ยาวขึ้น เช่น SHA256 ก็จะสั้นกว่า SHA512 นั่นก็เพราะว่าเลขที่อยู่ด้านหลังนั้นเป็นตัวบอกว่า algorithm นั้นๆมีขนาดเท่าไหร่นั่นเองขอรับ เลขยิ่งเยอะยิ่งป้องกันได้ดี แต่ก็ต้องแลกมากับเวลาในการประมวลผลเช่นกัน ดังนั้นเราต้อง balance มันว่าแต่ละเรื่องควรจะใช้ขนาดไหนถึงจะเหมาะสมนั่นเองฮ๊าฟ \(ณ วันที่เขียนบทความนี้ SHA256 ถือว่าเป็นตัวที่ถูกแนะนำให้ใช้ แต่ในอนาคตก็ไม่แน่ผ่านไปอีกซักไม่กี่ปีก็อาจจะถือว่าไม่ปลอดภัยแล้วก็ได้ 😂\)

อะเชก็ถือว่าจบไปแล้วสำหรับ Hash เดี๋ยวถัดไปเราจะมาดูตัวที่มีความปลอดภัยมากกว่า Hash บ้างนั่นก็คือ HMAC กั๊ฟป๋ม 😎

{% hint style="success" %}
เกลียด ชอบ ถูกใจ อยากติดตาม อยากติชมแนะนำด่าทอ หรืออะไรก็แล้วแต่ \(ห้ามมายืมเงิน\) จิ้มลงมาที่เพจนี้ได้เลย [**Mr.Saladpuk**](https://www.facebook.com/mr.saladpuk) และจะเป็นประคุณอันล้นพ้นถ้ากด Like + Follow + Share ให้ด้วยขอรับ น้ำตาจิไหล 🥺
{% endhint %}

![](../../../.gitbook/assets/promptpay.png)

