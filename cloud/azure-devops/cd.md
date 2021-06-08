# ลองทำ Continuous Delivery \(CD\)

{% hint style="success" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นส่วนหนึ่งของคอร์ส [**👶 Azure DevOps**](https://saladpuk.gitbook.io/learn/cloud/azure-devops) ที่จะมาสอนการใช้งานทุกสิ่งที่ DevOps ควรจะต้องมี เพื่อให้เราสามารถทำ Feedback loop ได้ไวขึ้น ดังนั้นถ้าเพื่อนๆสนใจอยากดูว่ามันทำอะไรได้ก็กดไปอ่านที่บทความหลักได้เลยครัช
{% endhint %}

{% hint style="warning" %}
**คำเตือน**  
ถ้าเพื่อนๆต้องการทำตามบทความนี้ จะต้องมีโปรเจคใน **Azure DevOps** ที่มีการนำทำ Continuous Integration ไว้เรียบร้อยแล้ว แต่ถ้าใครยังไม่มีก็สามารถไปทำตามได้จากบทตัวนี้ก่อน [**ลองทำ Continuous Integration \(CI\)**](https://saladpuk.gitbook.io/learn/cloud/azure-devops/ci) _\*\*_แล้วค่อยกลับมาทำตามบทความนี้ก็ได้ครัช
{% endhint %}

หลังจากที่เรามีระบบ **Automation** ในการตรวจสอบความถูกต้องเวลาที่มีคนส่งงานเข้ามา โดยการใช้ **Continuous Integration** หรือ **CI** เรียบร้อยแล้ว ดังนั้นในรอบนี้เราก็จะลองทำระบบ Automation ให้มันเอางานของเราไปขึ้นบทเซิฟเวอร์ของเราดูบ้าง ดังนั้นไปดูกันเบย

## 🔥 Build Pipeline

ในการทำ Build pipeline ในรอบนี้จะเน้นไปที่เรื่องการเอา source code ของเราไป deploy ที่เซิฟเวอร์กันบ้าง ซึ่งเราเรียกขั้นตอนนี้ว่าการทำ **Continuous Delivery** หรือย่อๆว่า **CD** นั่นเอง ซึ่งเราสามารถเข้าไปจัดการ Build pipeline ได้จากเมนูด้านซ้ายมือในหมวดของ **Pipeline** นั่นเอง ดังนั้นจิ้มมันไปซะ

{% hint style="info" %}
**Continuous Delivery**  
การทำงานของ **CD** ตามปรกติคือเมื่อเรา push งานขึ้นมาที่เซิฟเวอร์แล้ว ระบบก็จะนำมา build test ต่างๆตามสิ่งที่เรากำหนดไว้ใน **Continuous Integration \(CI\)** ซึ่งถ้ามัน build ผ่านหมดแล้ว เราก็จะให้มันทำการ deploy ลงไปที่เซิฟเวอร์แบบดัตโนมัตินั่นเอง แต่โดยปรกติเราจะ deploy ไปที่ตัว **Test Environment** หรือ **Staging Environment** เพื่อตรวจเช็คความถูกต้องต่างๆก่อน เช่นดูว่า features ใหม่ถูกต้องไหม **UX** ดีพอหรือยัง บลาๆ ซึ่งเมื่อเราทดสอบทุกอย่างจนหนำใจแล้ว เราก็จะทำการสั่ง **CD** ไปยัง **Production Environment** ในขั้นตอนสุดท้ายนั่นเอง
{% endhint %}

![](../../.gitbook/assets/image%20%28528%29%20%281%29.png)

ในการทำ **Continuous Delivery** นั้นมันจะอยู่ในหมวดย่อยของ **Pipelines** นั่นก็คือเมนู **Releases** นั่นเอง

![](../../.gitbook/assets/image%20%2833%29.png)

### สร้าง Continuous Delivery

เจ้าโปรเจคของเราพอดีว่ามันพึ่งถูกสร้างขึ้นมาใหม่ มันก็จะยังไม่มี **Release** อะไรเลย ดังนั้นที่ด้านขวามือกดจงกด **`New pipeline`** ลงไปซะ

![](../../.gitbook/assets/image%20%28479%29%20%281%29.png)

ถัดมาเขาก็จะโชว์ให้เราดูว่าเราจะตั้งวิธีการ deploy ไปที่ไหนบ้าง โดยเขาจะแบ่งออกเป็น 2 เรื่องคือ **Artifacts** และ **Stages** นั่นเอง

![](../../.gitbook/assets/image%20%28895%29.png)

### การตั้งค่า Stages

ในส่วนของ **Stages** คือการกำหนดว่า เราจะเอางานของเราไปทำการ deploy ไปที่เซิฟเวอร์ หรือ Environment ไหนบ้าง โดยในขั้นตอนนี้ที่ด้านขวาสุดเขาจะมี **Template** ในการ deploy ให้เราเลือกหลายแบบเลย ซึ่งในตัวอย่างนี้ผมจะ deploy ตัวเว็บไปบนคลาว์ของ Microsoft ละกัน ดังนั้นก็เลือก template เป็น **Azure App Service deployment** ได้เบย

![](../../.gitbook/assets/image%20%2898%29.png)

หลังจากที่เลือก template เรียบร้อยแล้ว ถัดไปเราก็จะต้องไปกำหนดค่าต่างๆว่าเราจะ deploy ไปที service ตัวไหนนั่นเอง โดยการกดที่ **Task** ตามรูปได้เลย

![](../../.gitbook/assets/image%20%28919%29.png)

ในขั้นตอนตั้งค่านี้ เราจะต้องเลือก subscription ที่เราจะทำงานด้วยเสียก่อน โดยการเลือก **Subscription** ตามรูปด้านล่างเลย

{% hint style="info" %}
**แนะนำให้อ่าน**  
สำหรับเพื่อนๆที่ยังไม่เคยสมัครคลาว์ของ Microsoft ก็สามารถไปสมัครได้ตามบทความด้านล่างนี้เลยครัช \(ฟรี + ได้สิทธิ์ประโยชน์เยอะม๊วกๆ รายละเอียดอ่านได้ในลิงค์เลย\) [**สมัคร Microsoft Azure**](https://saladpuk.gitbook.io/learn/cloud/azure101/register)\*\*\*\*
{% endhint %}

![](../../.gitbook/assets/image%20%28826%29.png)

หลังจากที่เลือก subscription ไปเรียบร้อยแล้วถัดไปเราก็จะต้องให้สิทธิ์ในการเข้าใช้งาน โดยการกดที่ปุ่ม **`Authorize`** ตามรูปเบย

![](../../.gitbook/assets/image%20%28501%29.png)

ทำการ Login เพื่อมอบสิทธิ์โลด

![](../../.gitbook/assets/image%20%28121%29.png)

ถัดไปก็เลือก Service ที่เราจะ deploy ขึ้นไป ซึ่งในตัวอย่างผมได้ web service ไว้บนคลาว์ของผมแล้วชื่อว่า _saladpuk-cd_ ดังนั้นในช่อง **App service name** ผมก็จะเลือกใช้ตัวนี้นั่นเอง

{% hint style="info" %}
**แนะนำให้อ่าน**  
สำหรับเพื่อนๆที่ยังไม่ได้สร้าง **Web App** บนคลาว์ สามารถเข้าไปดูวิธีการสร้างได้จากบทความด้านล่างนี้ได้เลยครับ [**สร้างเว็บตัวแรกบนคลาว์กัน**](https://saladpuk.gitbook.io/learn/cloud/azure101/website)\*\*\*\*
{% endhint %}

![](../../.gitbook/assets/image%20%28647%29.png)

เรียบร้อยครับสำหรับการตั้งค่าว่าเราจะเอางานไปขึ้นที่เซิฟเวอร์ตัวไหน ตามรูปเลย

![](../../.gitbook/assets/image%20%2851%29.png)

### การตั้งค่า Artifacts

ในส่วนของ **Artifacts** คือการเลือกว่าเราจะเอาไฟล์ไหนไปขึ้นเซิฟเวอร์บ้าง โดยเราสามารถกดได้จากเมนูด้านบนที่ชื่อว่า **Pipeline** แล้วเลือก **Artifacts** ตามรูปเลยครัช

![](../../.gitbook/assets/image%20%28514%29.png)

เมื่อเลือกแล้วเขาจะเปิดเมนูด้านขวาขึ้นมาให้เราเลือกว่า ไฟล์ที่จะเอาไป deploy นั้นอยู่ที่ไหน ซึ่งในตัวอย่างนี้ผมจะใช้ไฟล์ที่ได้จากการทำ **Continuous Integration \(CI\)** มาใช้นั่นเอง ดังนั้นเลือกตามรูปได้เลย

![](../../.gitbook/assets/image%20%28106%29.png)

ซึ่งเมื่อเลือกเสร็จเรียบร้อยเราก็กดปุ่ม `Add` ที่ด้านล่างสุดท้ายได้เลย

{% hint style="info" %}
**Default Version**  
เราสามารถเลือกได้ว่าเราจะเอาไฟล์จากการทำ **Continuous Integration \(CI\)** ในรูปแบบไหน ซึ่งแบบมาตรฐานคือเขาจะเลือกตัวล่าสุดไว้ให้เสมอ ซึ่งในจุดนี้เราสามารถปรับเลือก version ของเราได้เองตามที่ชอบใจเลย \(รายละเอียดขอแยกไปไว้บทอื่นนะฮั๊ฟ\)
{% endhint %}

![](../../.gitbook/assets/image%20%28527%29.png)

### สั่ง Save การตั้งค่า

ถ้าเราตั้งค่าทุกอย่างจนพอใจแล้ว หรืออยากบันทึกการตั้งค่านี้ไว้แล้วค่อยกลับมาแก้ไขภายในก็สามารถทำได้โดยการกดปุ่ม **`Save`** ที่มุมบนขวาได้เลย

![](../../.gitbook/assets/image%20%2811%29.png)

สุดท้ายเขาก็จะให้เรา commit นิดหน่อย ซึ่งจะใส่ comment ว่าเราทำอะไรลงไปเพื่อเตือนความจำตอนที่เราจะย้อน state กลับได้ง่ายๆเอาไว้ก็ดี สุดท้ายก็กด **`OK`** ไปครับ

![](../../.gitbook/assets/image%20%28957%29.png)

### สั่ง Deploy

เมื่อเราตั้งค่าครบทุกอย่างจนพอใจแล้ว ที่ปุ่มมุมบนขวา `...` เมื่อกดเข้าไปให้เลือก **`Release`** เพื่อให้ระบบทำการเริ่มกระบวนการ Deploy งานไปขึ้นเซิฟเวอร์ได้เลย ตามรูปด้านล่าง

![](../../.gitbook/assets/image%20%28348%29.png)

คราวนี้เราก็เลือก State ที่เราอยากจะทำการ Release ได้เลย ซึ่งในตัวอย่างเราสร้างไว้ชื่อว่า `State 1` ก็เลือกมันไปแล้วก็กดปุ่ม **`Create`** ได้เลย

![](../../.gitbook/assets/image%20%28684%29.png)

การตั้ง Deploy ของเราก็เสร็จเรียบร้อยแล้วครับ ซึ่งการตั้งค่าตัวนี้จะเป็นการทำ Manual Approve นั่นหมายความว่า ถ้าจะทำการ deploy เราจะต้องมานั่งกด Approve เพื่อสั่งให้มันเริ่ม deploy ลงไปนั่นเอง โดยเราสามารถเข้าไปกด Manual Approve ได้โดยการกดที่ชื่อ Deployment ตามรูปเลย

{% hint style="info" %}
**Manual Approve**  
เป็นมาตรฐานที่ดีสำหรับการ Deploy งานไปยัง Environments ที่ค่อนข้าง sensitive เช่น Production Environment เพราะเราควรจะต้องมีการตรวจสอบงานพวกนี้มาแล้วในระดับหนึ่ง ไม่ใช่เขียนโค้ดเสร็จก็เอาโค้ดตัวนั้นมา deploy เลยนั่นเอง
{% endhint %}

![](../../.gitbook/assets/image%20%28969%29.png)

เมื่อกดเข้ามาแล้วเราก็จะเจอปุ่ม Deploy เพื่อเป็นการยืนยันว่าเราจะเอางานไปใช้บนเซิฟเวอร์นั้นจริงๆ ซึ่งในตัวอย่างผมไม่มีอะไรที่ต้องกังวลอยู่แล้วเลยสามารถกด Deploy ได้เลยครับ

![](../../.gitbook/assets/image%20%28612%29.png)

อย่าลืมกด commit + comment ให้เรียบร้อยด้วย

![](../../.gitbook/assets/image%20%28994%29.png)

เรียบร้อยครับตอนนี้ก็แค่ไปดื่มกาแฟสักแก้รอมันเอางานขึ้นจนเสร็จนั่นเอง

![](../../.gitbook/assets/image%20%2842%29.png)

{% hint style="warning" %}
**ทำตามแล้วไม่ได้**  
สำหรับใครที่ทำตามทุกอย่างแล้วมันไม่ผ่านแล้วเขาแจ้งเตือนประมาณด้านล่างนี้ ก็ไม่ต้องตกใจนะครับ วิธีแก้อยู่ในหัวข้อ **แก้ Build Pipeline** ด้านล่างครับ

Error: No package found with specified pattern: D:\a\r1\a\*_\_.zip
{% endhint %}

เรียบร้อยครับเว็บของผมก็พร้อมใช้งานได้ตามปรกติแล้ว เย่

![https://saladpuk-cd.azurewebsites.net](../../.gitbook/assets/image%20%28982%29.png)

ถ้าเรากลับไปดูที่ **Build &gt; Summary** ที่เราทำไว้ ก็จะเห็นว่าตัว Build ของเราถูกส่งขึ้นไป Deploy ทั้งหมดกี่ครั้งแล้ว สำหรับหรือล้มเหลวยังไงบ้าง

![](../../.gitbook/assets/image%20%28498%29.png)

### แก้ไข Continuous Delivery

เมื่อทำการตั้งค่าทุกอย่างเสร็จแล้ว แต่เราอยากแก้ไขเพิ่มเติมก็สามารถทำได้โดยการกดที่ **Release** เหมือนเดิม

![](../../.gitbook/assets/image%20%2833%29.png)

แล้วก็เลือกที่ปุ่ม `Edit` ที่ด้านบนขวาเพื่อทำการแก้ไขได้เลยครัช

![](../../.gitbook/assets/image%20%28935%29.png)

## 🔥 แก้ Build Pipeline

สำหรับเพื่อนๆที่ทำตามผมมาตั้งแต่ทำ Continuous Integration \(CI\) จนถึง Continuous Delivery \(CD\) ในบทนี้แล้วก็ยังไม่ได้ ก็ไม่ต้องตกใจนะครับ เพราะในตัวอย่างการทำ CI มันจะต้องใส่ขั้นตอนเพิ่มเข้าไปอีกนิดหน่อยเพื่อให้มันทำงานได้ครับ โดยให้เรากลับไปที่ **Pipeline &gt; Build** ครับ \(ส่วนสาเหตุจะอธิบายไว้ในขั้นตอนด้านล่างนะจ๊ะ\)

![](../../.gitbook/assets/image%20%28426%29%20%281%29.png)

แล้วทำการกด **`Edit`** ที่มุมบนขวาได้เลย

![](../../.gitbook/assets/image%20%28443%29%20%281%29.png)

เราจะเห็นโค้ด .yaml ที่เอาไว้กำหนดขั้นตอนการทำงานต่างๆ ซึ่งภายในนั้นมันจะมีแค่สั่งให้เราใช้คำสั่งมาตรฐานอย่างเดียวเท่านั้น ซึ่งตามปรกติถ้าเราจะเอางานไป deploy เราจะต้องใช้คำสั่งในการ publish เพิ่มเข้าไปด้วย

![](../../.gitbook/assets/image%20%28427%29.png)

ดังนั้นในส่วน steps ที่วงไว้สีแดงๆ เราจะลบมันทิ้งทั้งหมด แล้วลองเขียนใหม่ให้เข้าใจง่ายๆดูนะครับ โดยเอาโค้ดแต่ละขั้นตอนด้านล่างไปค่อยๆวางลงโซนที่เราลบไป ตามขั้นตอนด้านล่างนี้เลย

### 1.สั่ง Restore Packages ต่างๆ

```yaml
- task: DotNetCoreCLI@2
  displayName: Restore
  inputs:
    command: 'restore'
    feedsToUse: 'select'
```

### 2.สั่ง Build projects

```yaml
- task: DotNetCoreCLI@2
  displayName: Build
  inputs:
    command: build
    projects: '**/*.csproj'
    arguments: '--configuration Release'
```

### 3.สั่งรันเทส

```yaml
- task: DotNetCoreCLI@2
  displayName: Test
  inputs:
    command: test
    projects: '**/*Tests/*.csproj'
    arguments: '--configuration $(buildConfiguration)'
```

### 4.สั่ง Publish และสร้างไฟล์

```yaml
- task: DotNetCoreCLI@2
  displayName: Publish
  inputs:
    command: publish
    publishWebProjects: True
    arguments: '--configuration $(BuildConfiguration) --output $(Build.ArtifactStagingDirectory)'
    zipAfterPublish: True

# this code takes all the files in $(Build.ArtifactStagingDirectory) and uploads them as an artifact of your build.
- task: PublishBuildArtifacts@1
  inputs:
    pathtoPublish: '$(Build.ArtifactStagingDirectory)' 
    artifactName: 'myWebsiteName'
```

เรียบร้อยละครับ สุดท้ายเราจะได้โค้ดทั้งหมดออกมาหน้าตาประมาณนี้

```yaml
# ASP.NET Core
# Build and test ASP.NET Core projects targeting .NET Core.
# Add steps that run tests, create a NuGet package, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/dotnet-core

trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'

steps:
- task: DotNetCoreCLI@2
  displayName: Restore
  inputs:
    command: 'restore'
    feedsToUse: 'select'
- task: DotNetCoreCLI@2
  displayName: Build
  inputs:
    command: build
    projects: '**/*.csproj'
    arguments: '--configuration Release'
- task: DotNetCoreCLI@2
  displayName: Test
  inputs:
    command: test
    projects: '**/*Tests/*.csproj'
    arguments: '--configuration $(buildConfiguration)'
- task: DotNetCoreCLI@2
  displayName: Publish
  inputs:
    command: publish
    publishWebProjects: True
    arguments: '--configuration $(BuildConfiguration) --output $(Build.ArtifactStagingDirectory)'
    zipAfterPublish: True
# this code takes all the files in $(Build.ArtifactStagingDirectory) and uploads them as an artifact of your build.
- task: PublishBuildArtifacts@1
  inputs:
    pathtoPublish: '$(Build.ArtifactStagingDirectory)' 
    artifactName: 'myWebsiteName'
```

ถ้าทุกอย่างพร้อมแล้วก็กดปุ่ม **`Save`** ที่ด้านบนขวาได้เลย

![](../../.gitbook/assets/image%20%28291%29.png)

หลังจากนั้นก็กด **`Run`** ตามปรกติครับ

![](../../.gitbook/assets/image%20%28762%29.png)

ขั้นตอนจะเพิ่มขึ้นมาหน่อยนึง แต่ถ้าอ่านแล้วก็จะเข้าใจได้ว่ามันทำอะไร เมื่อมองกลับไปที่ไฟล์ .yaml นั่นเอง

![](../../.gitbook/assets/image%20%28562%29.png)

{% hint style="info" %}
**แนะนำให้อ่าน**  
สำหรับรายละเอียดการสร้าง yaml เพื่อทำ run steps นั้นสามารถอ่านได้จากลิงค์หลักของ Microsoft ได้เลย ส่วนถ้าเพื่อนๆใช้ภาษาอื่นในการเขียนเว็บก็สามารถเลือกภาษาที่ตัวเองใช้ได้จากเมนูด้านซ้ายมือเลย  
[Microsoft document - Build, test, and deploy .NET Core apps](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/dotnet-core?view=azure-devops)
{% endhint %}

หลังจากที่ได้ลอง **CI/CD** ไปเรียบร้อยแล้ว คราวนี้ในเรื่องของการวางแผนงานต่างๆเช่น Product Backlog หรืองานในแต่ละ **Iteration/Sprint** ใครจะต้องทำอะไรยังไงบ้าง ดังนั้นกด **`Next`** เพื่อไปดูเรื่อง **Kanban Board** กันเลยครัช

