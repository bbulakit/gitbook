# Dependency-Inversion Principle

## 👑 หัวใจหลักของ Dependency-Inversion Principle \(DIP\)

> High level modules should not depend on low level modules; both should depend on abstractions. Abstractions should not depend on details. Details should depend upon abstractions.

**"ของที่เป็น High level module ไม่ควรไปผูกติดกับ Low level module และทั้งสองควรรู้จักกันในรูปแบบ abstraction เท่านั้น"** กับ **"Abstraction ไม่ควรรู้รายละเอียดการทำงาน แต่โค้ดที่ทำงานที่แท้จริงต้องทำตาม abstraction ที่วางไว้"** อืมมม นี่มันคืออะไรกันแฟร๊ะ?

{% hint style="info" %}
**High level module**  
คือโค้ดที่รับผิดชอบดูแลภาพรวมของระบบนั้นๆ เพื่อให้ระบบในส่วนที่มันดูแลเป็นไปตามความต้องการ ซึ่งภายในตัวมันเองจะไปเรียกใช้ Low level module ต่างๆมาทำงานอีกที  
\(มองง่ายๆว่ามันคือ**คนคุมงาน** คนคุมงานจะไม่ลงมาโบกปูนก่อเสาเอง แต่เขาจะดูภาพรวมว่าต้องทำอะไรงานถึงจะเสร็จ และในการสร้างตึกซักหลังก็จะมีคนคุมงานหลายๆคน เพื่อคอยคุมงานแต่ละส่วน\)
{% endhint %}

{% hint style="info" %}
**Low level module**  
คือโค้ดที่ต้องลงไปมำงานจริงๆ เช่นจะเขียนไฟล์ มันก็จะต้องเรียกใช้คำสั่งเขียนไฟล์ให้ถูกต้อง  
\(มองง่ายๆว่ามันคือ**คนลงมือทำงาน** ซึ่งเจ้านี่แหละคือคนที่จะลงไปโบกปูนก่อนเสาจริงๆ และในการสร้างตึกก็จะมีคนทำงานในแต่ละเรื่องๆไป เช่นเรื่องโบกปูนฉาบปูน เรื่องระบบไฟฟ้า เรื่องระบบน้ำ ฯลฯ หรือก็คือ **กรรมกร** ดีๆนี่เอง\)
{% endhint %}

## ❓ ทำไมต้องห้าม High level module ไปผูกติดกับ Low level module ?

High level module มีหน้าที่แค่ดูแลภาพรวมอย่างเดียว ถ้าเกิดว่ามันไปวุ่นวายกับ Low level module แล้วปัญหาที่จะตามมาคือ **เวลาที่เราเปลี่ยน Low level module เป็นตัวอื่น เราก็ต้องไปแก้ High level module ด้วย**นั่นเอง และ **High level module จะไม่สามารถนำมา Reuse ได้เลย เพราะมันต้องทำงานกับ Low level module ตัวนี้เท่านั้น**

## 🥶 ตัวอย่าง High level module ไปผูกติดกับ Low level module

สมมุติว่าผมอยากจะเขียนโปรแกรมรีโมตที่มีปุ่มเดียวเอาไว้สั่ง เปิด/ปิด ทีวีได้ เราจะออกแบบยังไงดี ? ... ซึ่งโดยทั่วไปเราก็จะออกแบบกันมาประมาณภาพด้านล่างนี้

![](../../.gitbook/assets/image%20%28990%29.png)

จากในรูป **High level module คือ Remote** และ **Low level module คือ Television** ซึ่งตอนนี้มันผิดกฎของ **DIP** อยู่ เพราะ Remote มันไปทำงานกับ Television เลยตรงๆ

{% hint style="danger" %}
**ปัญหา**  
เมื่อมีความต้องการใหม่ๆเกิดขึ้น หรือคลาส Television มีการเปลี่ยนแปลง มันอาจจะส่งผลให้เราต้องกลับไปแก้ไขคลาส Remote ด้วย ทั้งๆที่รีโมทไม่ทำอะไรผิดเลย
{% endhint %}

หรืออยู่มาวันนึง ก็มีอีกปุ่มให้เจ้ารีโมทสามารถเปลี่ยนไปควบคุมอย่างอื่นได้ด้วย เช่น เปิด/ปิด **ประตู** เราก็จะเจอปัญหาในการออกแบบทันทีว่า **รีโมทจะทำงานร่วมกับ ทีวี และ ประตูยังไง?** เพราะทั้ง _methods_ และ _property_ ของทีวีและประตูไม่เหมือนกัน

![](../../.gitbook/assets/image%20%28770%29.png)

{% tabs %}
{% tab title="Remote.cs" %}
```csharp
public class Remote
{
    // จะเก็บ object ที่กำลังทำงานด้วยยังไง ?
    private Television target;

    public void SwitchToggle()
    {
        // จะเขียนให้ทำงานร่วมกับ ประตูยังไง
        if (target.IsTurnedOn)
        {
            target.TurnOff();
        }
        else
        {
            target.TurnOn();
        }
    }

    public void SwitchTarget()
    {
        // จะทำการเปลี่ยนจาก ทีวี เป็น ประตูได้ยังไง
    }
}
```
{% endtab %}

{% tab title="Television.cs" %}
```csharp
public class Television
{
    public bool IsTurnedOn { get; set; }

    public void TurnOn()
    {
        IsTurnedOn = true;
        Console.WriteLine("Tv: Turned ON");
    }

    public void TurnOff()
    {
        IsTurnedOn = false;
        Console.WriteLine("Tv: Turned OFF");
    }
}
```
{% endtab %}

{% tab title="Door.cs" %}
```csharp
public class Door
{
    public bool IsOpening { get; set; }

    public void Open()
    {
        IsOpening = true;
        Console.WriteLine("Door: Opened");
    }

    public void Close()
    {
        IsOpening = false;
        Console.WriteLine("Door: Closed");
    }
}
```
{% endtab %}
{% endtabs %}

## 😒 **ควรออกแบบยังไงดี**

หลักการแก้ไขเรื่อง **DIP** เขาให้แก้ในเรื่องของ **Flow of control** โดยให้มันทำงานกลับด้านกัน แทนที่ High level module จะไปทำงานกับ Low level module ตรงๆ ก็กลับด้าน Flow of control ซะซึ่งมันจะทำให้ **High level module และ Low level module รู้จักกันในเชิง Abstraction เท่านั้น**

{% hint style="info" %}
**Flow of control**  
คือทิศทางของความสัมพันธ์ว่าใครควบคุม/ดูแลใคร เช่น ในภาพตัวอย่างด้านบนจะเห็นว่า **Remote มันเป็นฝ่ายไปควบคุม Television** โดยตรง
{% endhint %}

### 😟 การออกแบ**บที่ไม่ดี**

จากตัวอย่างเรื่องรีโมทด้านบน **Flow of control** ของมันไม่ดี เพราะ Remote ทำงานโดยตรงกับ ทีวี เลยทำให้ High level module เปลี่ยนไปทำงานร่วมกับ Low level module ได้ยาก

![](../../.gitbook/assets/image%20%28990%29.png)

### 😄 การออกแบบที่ควรเป็น

แทนที่ High level module จะทำงานตรงๆกับ Low level module เราก็**สร้าง Abstraction ของสิ่งที่ High level module อยากได้ขึ้นมา ส่วน Low level module ก็แค่ทำตาม abstraction** ที่ว่ามาก็พอ ตามภาพด้านล่าง จะเห็น Flow of control มันเปลี่ยนกลับด้านกันละ เพราะ Remote ไม่ได้ไปควบคุม Television กับ Door ตรงๆละ มันแค่รู้จักของที่มันต้องดูแลเป็นแค่ abstraction เท่านั้น

![](../../.gitbook/assets/image%20%28617%29%20%281%29.png)

{% tabs %}
{% tab title="IControllable.cs" %}
```csharp
public interface IControllable
{
    void Enable();
    void Disable();
    bool IsEnabled();
}
```
{% endtab %}

{% tab title="Television.cs" %}
```csharp
public class Television : IControllable
{
    public bool IsTurnedOn { get; set; }

    public void TurnOn()
    {
        IsTurnedOn = true;
        Console.WriteLine("Tv: Turned ON");
    }

    public void TurnOff()
    {
        IsTurnedOn = false;
        Console.WriteLine("Tv: Turned OFF");
    }

    public void Enable() => TurnOn();
    public void Disable() => TurnOff();
    public bool IsEnabled() => IsTurnedOn;
}
```
{% endtab %}

{% tab title="Door.cs" %}
```csharp
public class Door : IControllable
{
    public bool IsOpening { get; set; }

    public void Open()
    {
        IsOpening = true;
        Console.WriteLine("Door: Opened");
    }

    public void Close()
    {
        IsOpening = false;
        Console.WriteLine("Door: Closed");
    }

    public void Enable() => Open();
    public void Disable() => Close();
    public bool IsEnabled() => IsOpening;
}
```
{% endtab %}

{% tab title="Remote.cs" %}
```csharp
public class Remote
{
    private IList<IControllable> items;
    private IControllable target;

    public Remote(IList<IControllable> items)
    {
        this.items = new List<IControllable>(items);
        target = items.FirstOrDefault();
    }

    public void SwitchToggle()
    {
        if (target == null)
            return;

        if (target.IsEnabled())
        {
            target.Disable();
        }
        else
        {
            target.Enable();
        }
    }

    public void SwitchTarget()
    {
        var currentTargetIndex = items.IndexOf(target);
        const int FirstItemIndex = 0;
        var nextIndex = (++currentTargetIndex <= items.Count) ?
            currentTargetIndex : FirstItemIndex;
        target = items[nextIndex];
    }
}
```
{% endtab %}

{% tab title="Program.cs" %}
```csharp
var items = new List<IControllable>
{
    new Television(),
    new Door(),
};
var remote = new Remote(items);

remote.SwitchToggle();
remote.SwitchToggle();

remote.SwitchTarget();

remote.SwitchToggle();
remote.SwitchToggle();

/* 
* ผลลัพท์
* Tv: Turned ON
* Tv: Turned OFF
* Door: Opened
* Door: Closed
*/
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**ผลลัพท์**  
แค่ปรับให้ High level module กับ Low level module มันรู้จักกันผ่าน Abstraction เราก็จะ**สามารถเพิ่ม Low level module ใหม่ๆเข้าไปได้เรื่อยๆโดยที่ High level module ไม่มีผลกระทบเลย** อีกทั้ง High level module ยังสามารถนำไป Reuse กับเรื่องอะไรก็ได้ที่เป็น IControllable ได้ด้วย
{% endhint %}

## 🎯 บทสรุป

การเขียนโค้ดโดยปรกติเรามักจะทำ dependency ไปยัง concrete class โดยตรง ซึ่งมันจะทำให้เรา เพิ่ม/ลด concreate class ได้ยาก และโค้ดไม่สามารถ Reuse ได้ ดังนั้นให้กลับด้าน dependency เสียใหม่ ให้เล็งไปที่ abstraction แทน แล้วสิ่งที่เคยผูกมันกันไว้ก็จะคลายลง ทำให้เรา เพิ่ม/ลด Reuse อะไรต่างๆได้ง่ายขึ้นโดยที่ไม่ต้องกลับไปแก้โค้ดเก่า 🤠

