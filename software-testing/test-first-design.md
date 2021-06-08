---
description: โชว์พลังที่แท้จริงของ Test-Driven Development (TDD) ด้วยเกม OX
---

# 👦 Test-First Design

รอบนี้เราจะมาดูพลังที่แท้จริงในการใช้ **Test-Driven Development \(TDD\)** ว่ามันจะมาช่วยเรื่อง **Code Design** ได้ยังไง!! โดยใช้เกม **OX** เป็นโจทย์ในการเขียนโปรแกรม เพื่อให้เพื่อนๆได้ลองเอาไปศึกษาปรับใช้ดู แล้วจะรู้ว่าการเขียนเทสก่อนมันทรงพลังขนาดไหน

{% hint style="success" %}
**ความเข้าใจผิดกับการเขียนเทส**  
Developer หลายๆคนจะบ่นว่า _"การเขียนเทสทำให้งานช้า"_ เลยหลีกเลี่ยงไม่ยอมเขียนเทสก่อนเขียนโค้ด แต่ในขณะที่ครูระดับระดับตำนานและนักเขียนโค้ดระดับเทพทุกคนเขาเขียนเทสกันก่อนเขียนโค้ดทุกคน และงานส่วนใหญ่ก็ชี้ออกมาแล้วว่า **"การเขียนเทสก่อนสุดท้ายมันเร็วกว่าการไม่เขียนเทสเยอะมาก"**
{% endhint %}

## 🎮 Game Start !

### 1.Design scenarios

🧔 หลายๆคนพอรู้ว่าจะเขียนเกม OX ก็จะนึกภาพ class diagram หรือโค้ดต่างๆไว้ในหัวกันแล้วใช่ไหมล่ะ ?

![Class Diagram &#xE41;&#xE1A;&#xE1A;&#xE04;&#xE34;&#xE14;&#xE40;&#xE23;&#xE47;&#xE27;&#xE46; &#xE44;&#xE21;&#xE48;&#xE15;&#xE49;&#xE2D;&#xE07;&#xE44;&#xE1B;&#xE19;&#xE31;&#xE48;&#xE07;&#xE27;&#xE34;&#xE40;&#xE04;&#xE23;&#xE32;&#xE30;&#xE2B;&#xE4C;&#xE21;&#xE31;&#xE19;&#xE2B;&#xE23;&#xE2D;&#xE01;&#xE2D;&#xE48;&#xE32;&#xE19;&#xE15;&#xE48;&#xE2D;&#xE40;&#xE25;&#xE22;](../.gitbook/assets/image%20%28822%29.png)

🧔 แต่ปรกติการทำ **Test First ขั้นตอนแรกเราจะไม่เขียนโค้ด และไม่เขียน Diagrams กันนะ!!** แต่สิ่งที่เราจะเขียนเป็นตัวแรกคือสิ่งที่เขาเรียกกันว่า **เทสเคส หรือ Scenarios** นั่นเอง ซึ่งเทสเคสมันแบ่งออกเป็น 3 กลุ่มง่ายตามนี้

1. กลุ่มปรกติที่เห็นได้บ่อยๆ \(Normal cases\)
2. กลุ่มที่นานๆจะเจอที \(Alternative cases\)
3. กลุ่มที่เกิดสถานะการณ์แปลกๆในในโปรแกรม \(Exception cases\)

🧔 ถัดมา**เราจะต้องคิดเหตุการณ์ทั้งหมดที่จะเกิดขึ้นกับเกมของเราแล้วเอาไปใส่ในแต่ละกลุ่ม** ซึ่งเหตุการณ์ที่จะใส่จะต้องบอกรายละเอียดตั้งแต่ต้นจนจบว่า**จะทำอะไรและเกิดผลลัพท์อะไร**ด้วย มาเดี๋ยวจะลองใส่ตัวอย่างกลุ่มแรกให้

```text
กลุ่มปรกติที่เห็นได้บ่อยๆ (Normal cases)
------
1.ลง X ลงไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O
2.ลง O ลงไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X
```

🧔 ข้อดีในการทำแบบนี้คือ**เราสามารถเห็นข้อผิดพลาดได้เลยโดยที่ยังไม่ต้องเขียนโค้ดด้วยซ้ำ** เช่น กฎิกาของเกม OX จริงๆจะต้องให้ X ลงก่อน ดังนั้นกรณีที่ 2 จะต้องไม่มี เพราะ O ลงก่อนไม่ได้! ดังนั้นก็ลบข้อ 2 ออกซะ

```text
กลุ่มปรกติที่เห็นได้บ่อยๆ (Normal cases)
------
1.ลง X ลงไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O
```

{% hint style="warning" %}
**ป้องกันการเข้าใจผิดก่อนที่จะเขียนโค้ด**  
คุณคิดว่าไปไล่อ่านโค้ดที่เขาเขียนเพื่อตรวจว่าคนเขียนเข้าใจงานถูกหรือเปล่าได้ง่ายไหม? เมื่อเทียบกับไปไล่ตรวจจากเทสเคสก่อน อันไหนง่ายกว่ากัน ?
{% endhint %}

🧔 ตรงนี้เราจะคิดเหตุการณ์ของ **กลุ่มปรกติที่เห็นได้บ่อยๆ** กันก่อน เพราะคนใช้จะต้องเจอเคสนี้บ่อยสุดราวๆ 80% ดังนั้นถ้าเราไล่เก็บเคสนี้ก่อน มันหมายความว่าน่าจะไม่มี bug กับงานพื้นฐานของแอพเราแล้วนั่นเอง ปะลองไล่ดูว่ามีไรบ้าง

```text
กลุ่มปรกติที่เห็นได้บ่อยๆ (Normal cases)
------
1.ลง X ไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O
2.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวเท่านั้น ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X
3.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O
4.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X
5.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 2 ตัว แต่ X ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O
6.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 3 ตัวและ O 2 ตัว แต่ O ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X
7.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 2 ตัว และ X ทั้งหมดเรียงกัน ระบบให้ลงได้และประกาศว่า X ชนะพร้อมกับเพิ่มแต้มให้ X 1 คะแนน
8.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 3 ตัวและ O 2 ตัว และ O ทั้งหมดเรียงกัน ระบบให้ลงได้และประกาศว่า O ชนะ พร้อมกับเพิ่มแต้มให้ O 1 คะแนน
9.ลง X ไปในช่องว่างซึ่งบนกระดานมี X 4 ตัวและ O 4 ตัว แต่ X ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ และแจ้งว่าเกมเสมอ
```

{% hint style="success" %}
ไม่ต้องนั่งคิดให้มันครบทุกเคสก็ได้นะ เอาแค่ที่นึกออกก็พอ คิดออกเมื่อไหร่ค่อยมาเพิ่มอีกหลังก็ได้
{% endhint %}

🧔 เราก็จะได้เทสเคสของกรณีที่เกิดขึ้นบ่อยๆออกมาราวๆด้านบนนี้ ถัดไปเราก็จะลองมาคิด **กลุ่มนานๆจะเจอที** กันบ้าง เพราะมันจะเป็นการเพิ่มความมั่นใจว่าถ้าเกิดเคสที่นานๆจะเจอที อย่างน้อยมันก็จะทำงานได้นั่นเอง ซึ่งพอไปคิดคร่าวๆมาก็น่าจะได้ราวๆนี้ \(ไม่ต้องรีดสมองคิดให้คลุมทั้งหมดก็ได้ มาเติมเอาทีหลังเช่นเคยก็โอเคนะ\)

```text
กลุ่มที่นานๆจะเจอที (Alternative cases)
-----
1.ลง X ไปในช่องที่ไม่ว่าง ระบบไม่ให้ลงพร้อมแจ้งเตือน และยังคงเป็นตาของ X อยู่
2.ลง O ไปในช่องที่ไม่ว่าง ระบบไม่ให้ลงพร้อมแจ้งเตือน และยังคงเป็นตาของ O อยู่
```

🧔 ถัดไปก็ **กลุ่มที่เกิดสถานะการณ์แปลกๆในในโปรแกรม** ซึ่งจะเพิ่มความมั่นใจว่าถ้าเจออะไรแปลกๆ อย่างน้อยเราแอพเราก็น่าจะรับมือได้ในระดับนึงนั่นเอง ซึ่งคิดคร่าวๆก็จะได้ราวๆนี้

```text
กลุ่มที่เกิดสถานะการณ์แปลกๆในในโปรแกรม
-----
1.ลง X ไปในช่องว่างซึ่งบนกระดานมี X 1 ตัวเท่านั้น ระบบไม่ให้ลงพร้อมแจ้งเตือน และสลับเป็นตาของ O
2.ลง X ไปในช่องที่ไม่มีอยู่ในกระดาน ระบบไม่ให้ลงพร้อมแจ้งเตือน และยังคงเป็นตาของ X อยู่
```

🧔 จากที่ทำมาทั้งหมดเราก็จะได้ เทสเคส ของตัวโปรแกรมอย่างง่ายๆออกมาแล้ว ซึ่งหน้าตาก็ประมาณนี้

```text
กลุ่มปรกติที่เห็นได้บ่อยๆ (Normal cases)
-----
1.ลง X ไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O
2.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวเท่านั้น ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X
3.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O
4.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X
5.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 2 ตัว แต่ X ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O
6.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 3 ตัวและ O 2 ตัว แต่ O ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X
7.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 2 ตัว และ X ทั้งหมดเรียงกัน ระบบให้ลงได้และประกาศว่า X ชนะพร้อมกับเพิ่มแต้มให้ X 1 คะแนน
8.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 3 ตัวและ O 2 ตัว และ O ทั้งหมดเรียงกัน ระบบให้ลงได้และประกาศว่า O ชนะ พร้อมกับเพิ่มแต้มให้ O 1 คะแนน
9.ลง X ไปในช่องว่างซึ่งบนกระดานมี X 4 ตัวและ O 4 ตัว แต่ X ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ และแจ้งว่าเกมเสมอ

กลุ่มที่นานๆจะเจอที (Alternative cases)
-----
10.ลง X ไปในช่องที่ไม่ว่าง ระบบไม่ให้ลงพร้อมแจ้งเตือน และยังคงเป็นตาของ X อยู่
11.ลง O ไปในช่องที่ไม่ว่าง ระบบไม่ให้ลงพร้อมแจ้งเตือน และยังคงเป็นตาของ O อยู่

กลุ่มที่เกิดสถานะการณ์แปลกๆในในโปรแกรม
-----
12.ลง X ไปในช่องว่างซึ่งบนกระดานมี X 1 ตัวเท่านั้น ระบบไม่ให้ลงพร้อมแจ้งเตือน และสลับเป็นตาของ O
13.ลง X ไปในช่องที่ไม่มีอยู่ในกระดาน ระบบไม่ให้ลงพร้อมแจ้งเตือน และยังคงเป็นตาของ X อยู่
```

### 2.Testable code

🧔 ถัดมาเราก็จะเอา **เทสเคส** จากขั้นตอนที่ 1 มาแปลงเป็นโค้ดที่เอาไว้ทดสอบโปรแกรมของเรา โดยเราจะเอามาทีละข้อ ไล่จากบนลงล่างเลย ดังนั้นข้อที่ 1 ของเราคือ

`1.ลง X ไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O`

เราก็จะเขียนเทสให้กับข้อนี้ก่อน ได้ตามนี้ \(ในตัวอย่างผมใช้ภาษา C\# กับ xUnit นะครับ\)

{% tabs %}
{% tab title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง X ไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O")]
public void PlaceXWhenBoardIsEmpty()
{
    var boardGame = new BoardGame();
    var canPlace = boardGame.Place("X", 0, 0);
    Assert.True(canPlace);
    Assert.Equal("O", boardGame.CurrentTurn);
    Assert.Null(boardGame.GetWinner());
}
```
{% endtab %}

{% tab title="BoardGame.cs" %}
```csharp
public string CurrentTurn { get; set; }

public bool Place(string symbol, int row, int column)
{
    throw new NotImplementedException();
}

public string GetWinner()
{
    throw new NotImplementedException();
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
ตัวอย่างโค้ดมันจะมีชื่อไฟล์อยู่นะ ให้กดชื่อไฟล์เพื่อนดูโค้ดในไฟล์นั้น + ตัวอย่างโค้ดผมจะเอาเฉพาะที่สำคัญมาให้ดูนะ จะได้ focus ได้ถูกจุดไม่งั้น งง ตายเลย
{% endhint %}

🧔 ซึ่งพอ run test มันก็จะ Fail ครับ เพราะโค้ดใน `BoardGame.cs` ยังไม่ได้เขียนอะไรเลย ดังนั้นเราก็จะเขียนแบบง่ายที่สุดเพื่อให้มันผ่านเทส เราก็จะได้โค้ดประมาณนี้

{% code title="BoardGame.cs" %}
```csharp
public string CurrentTurn { get; set; }

public bool Place(string symbol, int row, int column)
{
    CurrentTurn = "O";
    return true;
}

public string GetWinner()
{
    return null;
}
```
{% endcode %}

{% hint style="info" %}
เขียนโค้ดให้น้อยที่สุดเท่าที่จะทำได้ เพื่อให้เทสผ่าน เพราะเราจะได้โค้ดที่ไม่ซับซ้อนอะไรเลยออกมาก่อน
{% endhint %}

🧔 เย่เทสผ่านละ!! ต่อมาเราก็จะเอาเคสตัวถัดไปเข้ามาเพิ่ม ซึ่งตัวถัดไปก็คือ

`2.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวเท่านั้น ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X`

เราก็จะเขียนเทสเคสให้ตัวนี้ต่อ ได้ตามนี้

{% tabs %}
{% tab title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง O ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวเท่านั้น ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X")]
public void PlaceOInEmptySlotWhenBoardHave_1X_0O()
{
    var boardGame = new BoardGame
    {
        Slots = new string[,]
        {
            { "X", null, null },
            { null, null, null },
            { null, null, null },
        }
    };
    var canPlace = boardGame.Place("O", 0, 1);
    Assert.True(canPlace);
    Assert.Equal("X", boardGame.CurrentTurn);
    Assert.Null(boardGame.GetWinner());
}
```
{% endtab %}

{% tab title="GameBoard.cs" %}
```csharp
public string[,] Slots { get; set; }
public string CurrentTurn { get; set; }

public bool Place(string symbol, int row, int column)
{
    CurrentTurn = "O";
    return true;
}

public string GetWinner()
{
    return null;
}
```
{% endtab %}
{% endtabs %}

🧔 ซึ่งพอเอาไป Run มันก็จะไม่ผ่าน เพราะคลาส `GameBoard` ยังเขียนแบบกากๆ ดังนั้นเราก็ต้องไปแก้ให้มันผ่านเคสนี้และเคสก่อนหน้าด้วยโดยเขียนให้ง่ายที่สุด

> ในรอบนี้ผมจะคำนวณว่าเป็นตาของใครจาก Slots โดยมันจะนับว่า Array ที่ไม่เป็น null เป็นเลขคู่หรือเลขคี่ เพราะถ้าเป็นเลขคู่แสดงว่าเป็นตาของ X แต่ถ้าเป็นเลขคี่แสดงว่าเป็นตาของ O นั่นเอง

{% code title="GameBoard.cs" %}
```csharp
public string[,] Slots { get; set; }
public string CurrentTurn { get; set; }

public BoardGame()
{
    Slots = new string[3, 3];
}

public bool Place(string symbol, int row, int column)
{
    Slots[row, column] = symbol;

    var counter = 0;
    foreach (var item in Slots)
    {
        if (item != null)
        {
            counter++;
        }
    }

    if (counter % 2 == 0)
    {
        CurrentTurn = "X";
    }
    else
    {
        CurrentTurn = "O";
    }

    return true;
}
```
{% endcode %}

🧔 ในตอนนี้ทั้งสองเคสก็จะทำงานผ่านหมดละ แต่โค้ดน่าเกลียดมาก ดังนั้นผมจะทำการปรับโค้ดให้อ่านง่ายขึ้นกว่าเดิมหน่อย ซึ่งเราเรียกขั้นตอนนี้ว่า **Refactor**

{% hint style="info" %}
**Refactor**  
คือการปรับโค้ดให้มันดียิ่งขึ้นกว่าเดิม ซึ่งการจะทำ Refactor ได้โค้ดจะต้องถูกเขียนเทสเคสคลุมไว้แล้ว เพราะ ถ้าเราปรับโค้ดไปแล้ว เราจะรู้ได้ไงว่าไอ้ที่ปรับไปมันยังทำงานถูกอยู่? ดังนั้นมันเลยต้องมีเทสเคสเป็นตัวช่วยเช็คว่ายังทำงานถูกเหมือนเดิมหรือเปล่านั้นเอง
{% endhint %}

🧔 ขั้นตอนแรกผมก็ Refactor ตัว loop ว่าเป็นเลขคู่เลขคี่หรือเปล่าในไฟล์ `GameBoard.cs` บรรทัดที่ 13~20 ซึ่งก็จะได้โค้ดใหม่ออกมาเป็นแบบนี้

{% code title="GameBoard.cs" %}
```csharp
public bool Place(string symbol, int row, int column)
{
    Slots[row, column] = symbol;

    var isEvenNumber = Slots.Cast<string>().Count(it => it != null) % 2 == 0;

    if (isEvenNumber)
    {
        CurrentTurn = "X";
    }
    else
    {
        CurrentTurn = "O";
    }

    return true;
}
```
{% endcode %}

🧔 ลอง Run test ละก็ผ่าน ดังนั้นผมก็จะ Refactor ต่อกับไฟล์เดิมนี่แหละ เพราะผมคิดว่า การตรวจว่าเป็นตาของ X หรือ O ในบรรทัดที่ 7~14 ยังเยิ่นเย้ออยู่ ซึ่งก็จะ Refactor ใหม่ออกมาได้เป็นแบบนี้

{% code title="GameBoard.cs" %}
```csharp
public bool Place(string symbol, int row, int column)
{
    Slots[row, column] = symbol;

    var isEvenNumber = Slots.Cast<string>().Count(it => it != null) % 2 == 0;
    CurrentTurn = isEvenNumber ? "X" : "O";

    return true;
}
```
{% endcode %}

🧔 อะเช Run test แล้วก็ผ่านอยู่ งั้นตอนนี้ไปเอาเทสเคสที่ 3 มาทำต่อบ้างดีกว่า ซึ่งมันเขียนไว้ว่า

`3.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O`

ดังนั้นผมก็จะเขียนเทสให้ตัวนี้ออกมาเป็นตามนี้

{% code title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง X ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O")]
public void PlaceXInEmptySlotWhenBoardHave_1X_1O()
{
    var boardGame = new BoardGame
    {
        Slots = new string[,]
        {
            { "X", "O", null },
            { null, null, null },
            { null, null, null },
        }
    };
    var canPlace = boardGame.Place("X", 1, 0);
    Assert.True(canPlace);
    Assert.Equal("O", boardGame.CurrentTurn);
    Assert.Null(boardGame.GetWinner());
}
```
{% endcode %}

🧔 แล้วก็ลอง Run test ก็จะพบว่ามันผ่านเหมือนกัน แต่สิ่งที่ผมเห็นแล้วน่ารำคาญคือเจ้าไฟล์ `BoardGameTest.cs` เพราะทุกครั้งที่ผมเขียนเทส มันจะดูเหมือนมันเขียนของเดิมซ้ำๆ ไม่เชื่อลองดูไฟล์เต็มๆมันดูนะ

{% code title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง X ไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O")]
public void PlaceXWhenBoardIsEmpty()
{
    var boardGame = new BoardGame();
    var canPlace = boardGame.Place("X", 0, 0);
    Assert.True(canPlace);
    Assert.Equal("O", boardGame.CurrentTurn);
    Assert.Null(boardGame.GetWinner());
}

[Fact(DisplayName = "ลง O ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวเท่านั้น ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X")]
public void PlaceOInEmptySlotWhenBoardHave_1X_0O()
{
    var boardGame = new BoardGame
    {
        Slots = new string[,]
        {
            { "X", null, null },
            { null, null, null },
            { null, null, null },
        }
    };
    var canPlace = boardGame.Place("O", 0, 1);
    Assert.True(canPlace);
    Assert.Equal("X", boardGame.CurrentTurn);
    Assert.Null(boardGame.GetWinner());
}

[Fact(DisplayName = "ลง X ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O")]
public void PlaceXInEmptySlotWhenBoardHave_1X_1O()
{
    var boardGame = new BoardGame
    {
        Slots = new string[,]
        {
            { "X", "O", null },
            { null, null, null },
            { null, null, null },
        }
    };
    var canPlace = boardGame.Place("X", 1, 0);
    Assert.True(canPlace);
    Assert.Equal("O", boardGame.CurrentTurn);
    Assert.Null(boardGame.GetWinner());
}
```
{% endcode %}

🧔 สังเกตุดีๆจะเห็นว่าข้างใน method ทั้ง 3 ตัวมันเขียนเกือบจะเหมือนกันเลย นั่นแสดงว่าทุกๆครั้งที่ผมจะเอาเทสเคสมาเพิ่ม ผมก็จะเขียนของที่คล้ายๆเดิมไปลงเรื่อยๆ ทำให้โค้ดมันรก ดังนั้นในรอบนี้ผมก็จะทำการ Refactor ในฝั่งของตัว Test บ้างละ

สิ่งที่ผมจะทำคือรวมทั้ง 3 method ให้กลายเป็น method เดียว แล้วส่ง parameter แบบต่างๆเข้าไปแทน ก็จะได้โค้ดออกมาเป็นแบบนี้

{% code title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง X ไปตอนที่กระดานว่าง ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O")]
public void PlaceXWhenBoardIsEmpty()
{
    var slots = new string[3, 3];
    verifyPlaceASymbolToEmptySpaceThenSystemMustAcceptTheRequest(slots, "X", 0, 0, "O");
}

[Fact(DisplayName = "ลง O ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวเท่านั้น ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X")]
public void PlaceOInEmptySlotWhenBoardHave_1X_0O()
{
    var slots = new string[,]
    {
        { "X", null, null },
        { null, null, null },
        { null, null, null },
    };
    verifyPlaceASymbolToEmptySpaceThenSystemMustAcceptTheRequest(slots, "O", 0, 1, "X");
}

[Fact(DisplayName = "ลง X ไปในช่องว่าซึ่งบนกระดานมี X 1 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O")]
public void PlaceXInEmptySlotWhenBoardHave_1X_1O()
{
    var slots = new string[,]
    {
        { "X", "O", null },
        { null, null, null },
        { null, null, null },
    };
    verifyPlaceASymbolToEmptySpaceThenSystemMustAcceptTheRequest(slots, "X", 1, 0, "O");
}

private void verifyPlaceASymbolToEmptySpaceThenSystemMustAcceptTheRequest(string[,] slots, string symbol, int row, int column, string expectedCurrentTurn)
{
    var boardGame = new BoardGame { Slots = slots };
    var canPlace = boardGame.Place(symbol, row, column);
    Assert.True(canPlace);
    Assert.Equal(expectedCurrentTurn, boardGame.CurrentTurn);
    Assert.Null(boardGame.GetWinner());
}
```
{% endcode %}

{% hint style="warning" %}
**Refactor**  
เวลาที่ทำ Refactor สามารถทำได้ทั้ง 2 ฝั่งทั้ง โค้ดที่ถูกเทส และ โค้ดที่เอาไว้เทส แต่เวลาทำ Refactor แต่ละรอบ **ต้องทำ Refactor ทีละฝั่งเท่านั้น ห้ามทำพร้อมกัน** ไม่งั้นเราจะไม่รู้ว่ามันพังเพราะอะไรกันแน่
{% endhint %}

🧔 แน่นอนถ้าผมเปลี่ยนเป็นแบบนี้ก็ต้องลอง Run test ให้มันผ่านด้วยเช่นกัน ซึ่งก็ผ่านตามที่คาดไว้ ดังนั้นผมก็จะเริ่มเอาเทสเคสที่ 4~6 ลงมาใส่ต่อเลย \(เพราะผมรู้ว่ามันก็ผ่านเหมือนกัน\)

`4.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X`

`5.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 2 ตัว แต่ X ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O`

`6.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 3 ตัวและ O 2 ตัว แต่ O ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X`

ซึ่งก็จะได้โค้ดออกมาตามนี้

{% code title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง O ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 1 ตัว ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X")]
public void PlaceOInEmptySlotWhenBoardHave_2X_1O()
{
    var slots = new string[,]
    {
        { "X", "O", null },
        { "X", null, null },
        { null, null, null },
    };
    verifyPlaceASymbolToEmptySpaceThenSystemMustAcceptTheRequest(slots, "O", 1, 1, "X");
}

[Fact(DisplayName = "ลง X ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 2 ตัว แต่ X ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ O")]
public void PlaceXInEmptySlotWhenBoardHave_2X_2O_ButNotConnectedTogather()
{
    var slots = new string[,]
    {
        { "X", "O", null },
        { "X", "O", null },
        { null, null, null },
    };
    verifyPlaceASymbolToEmptySpaceThenSystemMustAcceptTheRequest(slots, "O", 0, 2, "O");
}

[Fact(DisplayName = "ลง O ไปในช่องว่าซึ่งบนกระดานมี X 3 ตัวและ O 2 ตัว แต่ O ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ แต่ยังไม่มีผู้ชนะ และสลับเป็นตาของ X")]
public void PlaceOInEmptySlotWhenBoardHave_3X_2O_ButNotConnectedTogather()
{
    var slots = new string[,]
    {
        { "X", "O", null },
        { "X", "O", null },
        { null, "X", null },
    };
    verifyPlaceASymbolToEmptySpaceThenSystemMustAcceptTheRequest(slots, "O", 2, 0, "X");
}
```
{% endcode %}

🧔 จากนั้นเราก็จะเริ่มเอาเทสเคสที่ 7 มาทำต่อ ซึ่งมันเป็นเทสเคสแรกที่มีคนชนะ โดยมันเขียนไว้ว่า

`7.ลง X ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 2 ตัว และ X ทั้งหมดเรียงกัน ระบบให้ลงได้และประกาศว่า X ชนะพร้อมกับเพิ่มแต้มให้ X 1 คะแนน`

ดังนั้นผมก็จะเอาไปสร้างเทสเคสออกมาเป็นแบบนี้

{% tabs %}
{% tab title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง X ไปในช่องว่าซึ่งบนกระดานมี X 2 ตัวและ O 2 ตัว และ X ทั้งหมดเรียงกัน ระบบให้ลงได้และประกาศว่า X ชนะพร้อมกับเพิ่มแต้มให้ X 1 คะแนน")]
public void PlaceXInEmptySlotWhenBoardHave_2X_2O_WithConnectedTogather()
{
    var slots = new string[,]
    {
        { "X", "O", null },
        { "X", "O", null },
        { null, null, null },
    };
    var boardGame = new BoardGame { Slots = slots };
    var canPlace = boardGame.Place("X", 2, 0);
    Assert.True(canPlace);
    Assert.Equal("X", boardGame.GetWinner());
    Assert.Equal(0, boardGame.OScore);
    Assert.Equal(1, boardGame.XScore);
}
```
{% endtab %}

{% tab title="BoardGame.cs" %}
```csharp
public int OScore { get; set; }
public int XScore { get; set; }
public string[,] Slots { get; set; }
public string CurrentTurn { get; set; }

public BoardGame()
{
    Slots = new string[3, 3];
}

public bool Place(string symbol, int row, int column)
{
    Slots[row, column] = symbol;

    var isEvenNumber = Slots.Cast<string>().Count(it => it != null) % 2 == 0;
    CurrentTurn = isEvenNumber ? "X" : "O";

    return true;
}

public string GetWinner()
{
    return null;
}
```
{% endtab %}
{% endtabs %}

🧔 ซึ่งพอเอาไป Run test มันก็จะ Fail เพราะ `BoardGame.cs` ยังไม่ถูกเขียนการคำนวณว่าใครชนะ และ ยังไม่มีการจัดการเรื่องคะแนน ดังนั้นผมเลยต้องเขียนแบบง่ายที่สุดให้มันผ่าน ซึ่งก็จะออกมาเป็นแบบนี้

{% code title="BoardGame.cs" %}
```csharp
public string GetWinner()
{
    var firstRow = Slots[0, 0] + Slots[0, 1] + Slots[0, 2];
    var secondRow = Slots[1, 0] + Slots[1, 1] + Slots[1, 2];
    var thirdRow = Slots[2, 0] + Slots[2, 1] + Slots[2, 2];
    var firstColumn = Slots[0, 0] + Slots[1, 0] + Slots[2, 0];
    var secondColumn = Slots[0, 1] + Slots[1, 1] + Slots[2, 1];
    var thirdColumn = Slots[0, 2] + Slots[1, 2] + Slots[2, 2];
    var crossTop = Slots[0, 0] + Slots[1, 1] + Slots[2, 2];
    var crossBottom = Slots[2, 0] + Slots[1, 1] + Slots[0, 2];

    var allPossibilities = new string[]
    {
        firstRow, secondRow, thirdRow,
        firstColumn, secondColumn, thirdColumn,
        crossTop, crossBottom
    };

    foreach (var item in allPossibilities)
    {
        if (item.Length < 3)
        {
            continue;
        }

        if (item[0] == item[1] && item[0] == item[2])
        {
            var winnerSymbol = item[0].ToString();
            if (winnerSymbol == "X")
            {
                XScore++;
            }
            return winnerSymbol;
        }
    }

    return null;
}
```
{% endcode %}

{% hint style="info" %}
ไม่ต้องสนใจว่าโค้ดจะน่าเกลียดขนาดไหนขอแค่มันทำงานได้ก็ OK แล้ว แต่ถ้าเขียนแบบลัดได้ก็เขียนไปเลย ที่ผมเขียนแบบกากๆให้ดูเพราะอยากให้ดูการ Refactor
{% endhint %}

🧔 จากที่เขียนมามันก็ OK นะเพราะมัน Run test ผ่าน แต่โค้ดแบบว่าฝุดๆอ่ะ อ่านก็ยาก ถ้ามันผิดมานี่ผมคงขี้เกียจไปแก้มันแน่ ดังนั้นขอ Refactor มันหน่อยละกัน ซึ่งสิ่งที่ผมจะทำก็คือทำให้โค้ด บรรทัดที่ 3~17 อ่านแล้วเป็นภาษามนุษย์ขึ้นมาหน่อย ซึ่งก็จะได้ออกมาเป็นแบบนี้

{% code title="BoardGame.cs" %}
```csharp
public string GetWinner()
{
    var allPossibilities = getRowPossibilities()
        .Union(getColumnPossibilities())
        .Union(getCrossLinePossibilities());

    foreach (var item in allPossibilities)
    {
        if (item.Length < 3)
        {
            continue;
        }

        if (item[0] == item[1] && item[0] == item[2])
        {
            var winnerSymbol = item[0].ToString();
            if (winnerSymbol == "X")
            {
                XScore++;
            }
            return winnerSymbol;
        }
    }

    return null;
}

private IEnumerable<string> getRowPossibilities()
{
    return new string[]
    {
        $"{Slots[0,0]}{Slots[0,1]}{Slots[0,2]}",
        $"{Slots[1,0]}{Slots[1,1]}{Slots[1,2]}",
        $"{Slots[2,0]}{Slots[2,1]}{Slots[2,2]}",
    };
}

private IEnumerable<string> getColumnPossibilities()
{
    return new string[]
    {
        $"{Slots[0,0]}{Slots[1,0]}{Slots[2,0]}",
        $"{Slots[0,1]}{Slots[1,1]}{Slots[2,1]}",
        $"{Slots[0,2]}{Slots[1,2]}{Slots[2,2]}",
    };
}

private IEnumerable<string> getCrossLinePossibilities()
{
    return new string[]
    {
        $"{Slots[0,0]}{Slots[1,1]}{Slots[2,2]}",
        $"{Slots[2,0]}{Slots[1,1]}{Slots[0,2]}",
    };
}
```
{% endcode %}

🧔 อ่าเทสผ่านละ จะเห็นว่าผม Refactor การสร้าง possibilities ของเกม ให้เป็นภาษามนุษย์แล้ว คือบรรทัดที่ 3 อันเดียวจบ ส่วน method ที่โดนสร้างเพิ่มขึ้นมาปล่อยมันไปเลยเพราะหน้าที่มันสมบูรณ์ในตัวแล้ว \(ผมไม่ได้จะมาสอนเขียน LinQ นะดังนั้นขอปล่อยไว้แบบนี้แหละ\)

🧔 ถัดมาผมก็จะ Refactor บรรทัดที่ 7~25 เพื่อให้อ่านแล้วเข้าใจได้ง่ายขึ้น ว่ากำลังหาผู้ชนะ ซึ่งก็จะออกมาเป็นแบบโค้ดด้านล่างนี้

{% code title="BoardGame.cs" %}
```csharp
public string GetWinner()
{
    var allPossibilities = getRowPossibilities()
        .Union(getColumnPossibilities())
        .Union(getCrossLinePossibilities());

    const int MinimumRequiredCharacters = 3;
    var winnerSpot = allPossibilities
        .Where(it => it.Length == MinimumRequiredCharacters)
        .FirstOrDefault(it => it.All(c => c == it.First()));

    var winnerSymbol = winnerSpot?.First().ToString();

    const string XSymbol = "X";
    if (winnerSymbol == XSymbol)
    {
        XScore++;
    }

    return winnerSymbol;
}
```
{% endcode %}

🧔 เทสผ่านเช่นเคย ดังนั้นผมก็จะลองเอาเทสเคส 8 มาลงต่อเลยละกัน

`8.ลง O ไปในช่องว่าซึ่งบนกระดานมี X 3 ตัวและ O 2 ตัว และ O ทั้งหมดเรียงกัน ระบบให้ลงได้และประกาศว่า O ชนะ พร้อมกับเพิ่มแต้มให้ O 1 คะแนน`

{% code title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง O ไปในช่องว่าซึ่งบนกระดานมี X 3 ตัวและ O 2 ตัว และ O ทั้งหมดเรียงกัน ระบบให้ลงได้และประกาศว่า O ชนะ พร้อมกับเพิ่มแต้มให้ O 1 คะแนน")]
public void PlaceOInEmptySlotWhenBoardHave_3X_2O_WithConnectedTogather()
{
    var slots = new string[,]
    {
        { "X", "O", "X" },
        { "X", "O", null },
        { null, null, null },
    };
    var boardGame = new BoardGame { Slots = slots };
    var canPlace = boardGame.Place("O", 2, 1);
    Assert.True(canPlace);
    Assert.Equal("O", boardGame.GetWinner());
    Assert.Equal(1, boardGame.OScore);
    Assert.Equal(0, boardGame.XScore);
}
```
{% endcode %}

🧔 แต่มันก็จะยังไม่ผ่าน เพราะผมยังไม่ได้เขียน เพิ่มคะแนนให้กับ O เลย ดังนั้นก็ไปทำให้มันผ่านซะ

{% code title="BoardGame.cs" %}
```csharp
public string GetWinner()
{
    var allPossibilities = getRowPossibilities()
        .Union(getColumnPossibilities())
        .Union(getCrossLinePossibilities());

    const int MinimumRequiredCharacters = 3;
    var winnerSpot = allPossibilities
        .Where(it => it.Length == MinimumRequiredCharacters)
        .FirstOrDefault(it => it.All(c => c == it.First()));

    var winnerSymbol = winnerSpot?.First().ToString();

    const string XSymbol = "X";
    const string OSymbol = "O";
    if (winnerSymbol == XSymbol)
    {
        XScore++;
    }
    else if(winnerSymbol == OSymbol)
    {
        OScore++;
    }

    return winnerSymbol;
}
```
{% endcode %}

🧔 ผ่านเรียบร้อยแล้วนะ ตอนนี้กลุ่มปรกติก็เหลือแค่เคส 9 อันสุดท้ายละ

`9.ลง X ไปในช่องว่างซึ่งบนกระดานมี X 4 ตัวและ O 4 ตัว แต่ X ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ และแจ้งว่าเกมเสมอ`

{% tabs %}
{% tab title="BoardGameTest.cs" %}
```csharp
[Fact(DisplayName = "ลง X ไปในช่องว่างซึ่งบนกระดานมี X 4 ตัวและ O 4 ตัว แต่ X ทั้งหมดไม่ได้เรียงกัน ระบบให้ลงได้ และแจ้งว่าเกมเสมอ")]
public void PlaceXInEmptySlotWhenBoardHave_4X_4O_WithConnectedTogather()
{
    var slots = new string[,]
    {
        { "X", "O", "X" },
        { "X", "O", "O" },
        { "O", "X", null },
    };
    var boardGame = new BoardGame { Slots = slots };
    var canPlace = boardGame.Place("X", 2, 2);
    Assert.True(canPlace);
    Assert.Null(boardGame.GetWinner());
    Assert.Equal(0, boardGame.OScore);
    Assert.Equal(0, boardGame.XScore);
    Assert.True(boardGame.IsDraw);
}
```
{% endtab %}

{% tab title="BoardGame.cs" %}
```csharp
public bool IsDraw { get; set; } // มีแค่อันนี้พี่เพิ่มเข้ามา
```
{% endtab %}
{% endtabs %}

🧔 แล้วก็ Fail ตามที่คาด ดังนั้นก็ไปทำให้ผ่านครับ

{% code title="BoardGame.cs" %}
```csharp
public string GetWinner()
{
    // ...
    else if (winnerSymbol == OSymbol)
    {
        OScore++;
    }
    else
    {
        var anyEmptySpace = Slots.Cast<string>().Any(it => it == null);
        IsDraw = !anyEmptySpace;
    }

    return winnerSymbol;
}
```
{% endcode %}

🧔 เย่ผ่าน สุดท้ายเราจะเห็นว่าเทสที่ 9 มันทำให้เราต้องกลับไปแก้เทสเคสทุกตัวเพื่อเช็คว่าเกมมันจะต้องไม่เสมอนะ ดังนั้นฝากไปลองเล่นกันต่อดูนะครับ เย่ๆๆ หนีจากบทความอันแสนยาวนี้ได้แล้ว

## 🎯 บทสรุป

การทำ Test First Design หรือ TDD สิ่งที่เราได้คือคุณภาพของโค้ดที่ดีเพราะโค้ดเราจะถูกคลุมด้วยเทสเคสทั้งหมดแล้ว ทำให้เรามั่นใจได้ว่าถ้าเกิดเหตุการณ์ที่อยู่ในเคสเกิดขึ้น โปรแกรมมันสามารถทำงานได้แบบไม่มี bug ค่อนข้างแน่นอน และเรายังสามารถทำ Refactor เพื่อทำ Clean Code เมื่อไหร่ก็ได้อีกด้วย และโค้ดที่ได้ออกมาก็จะไม่มีการ ออกแบบที่เกินความจำเป็น เพราะทุกอย่างเป็น minimal หมดเลย ซึ่งของที่เป็น minimal นี่แหละสามารถแก้ไขหรือปรับไปเป็นโครงสร้างอื่นๆได้ง่ายที่สุด

### 👨‍🚀 หัวใจหลักในการทำ TDD

หัวใจในการทำ Test-Driven Development คือการทำ 3 เรื่องครับ Red - Green - Refactor หรือพูดง่ายๆคือเขียนเทสให้มันไม่ผ่านก่อน แล้วทำให้มันผ่าน สุดท้ายค่อยกลับมาทำให้โค้ดมัน Clean ขึ้น นั่นเอง ซึ่งพอทำแบบนี้โค้ดที่เราเขียนมันก็จะค่อยๆเก่งขึ้นไปเรื่อยๆ ข้อผิดพลาดก็จะน้อยลงไปเรื่อยๆเช่นกันครับ

![](../.gitbook/assets/image%20%28577%29%20%281%29.png)

{% hint style="info" %}
ต้องขอปรบมือให้เลยสำหรับคนที่อ่านตั้งแต่ต้นจนถึงตรงนี้ได้ สุดยอดกับความรักในการเขียนโค้ดจริงๆครับ
{% endhint %}

### 👨‍🚀 ดาวโหลดตัวอย่างทั้งหมด

โหลดได้จาก GitHub นี้เบย [https://github.com/saladpuk/demo-test-first](https://github.com/saladpuk/demo-test-first)

### 👨‍🚀 คอร์สเรื่องการทำ TDD แบบเต็มสูบ

{% page-ref page="tdd101/" %}

