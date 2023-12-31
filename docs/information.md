# 简介
## 本书简介
我们编写本书的目的是写下我们多年来在使用 Django 时学到的所有不成文的技巧、窍门和常见做法。  

在写作过程中，我们将自己视为抄写员，获取人们认为是常识的各种东西，并用简单的例子记录下来。  
## 关于我们的建议
与 Django 官方文档一样，本书介绍了如何使用 Django 进行操作，并通过代码示例说明了各种情况。

与 Django 文档不同，本书推荐特定的编码风格、模式和库的选择。虽然 Django 核心开发人员可能只会同意其中的部分选择，但请记住，我们的许多建议仅仅是：在使用 Django 多年后形成的个人建议。  

在本书中，我们提倡某些实践和技术，我们认为它们是最佳方法。我们还表达了对特定工具和库的个人偏好。  

有时，我们会反对我们认为是反模式的常见做法。对于大多数我们反对的地方
我们会尽量保持礼貌，尊重作者的辛勤工作。在极少数情况下，我们可能不会这么客气。这是为了帮助您避免危险的陷阱。  

我们会尽一切努力提出深思熟虑的建议，并确保我们的做法是合理和正确的。我们已经接受了来自我们非常尊敬的 Django 和 Python 核心开发人员的严厉、严肃的批评。我们请了
比一般的技术书籍更多的技术审核人员，我们投入了无数的小时来修改。尽管如此，错误或遗漏的可能性始终存在。也有可能出现比这里描述的更好的做法。


我们全力以赴，不断改进和完善本书，我们是认真的。如果您发现任何你不同意的做法，或者任何可以做得更好的地方，我们恳请你向我们提出改进建议。向我们发送反馈的最佳方式是在

<https://github.com/feldroy/two-scoops-of-django-3.x/issues>

请不要犹豫，告诉我们哪些地方可以改进。我们将积极采纳您的反馈意见。
## 开始之前
本书不是教程。如果您是 Django 的新手，本书会对您有所帮助，但大部分内容会对您构成挑战。要充分使用本书，您应了解 Python 编程语言，并至少阅读过官方的 Django 教程、整个 Django 速成教程 (feldroy.com/products/django-crash-course) 或其他类似的入门资源。具有面向对象编程的经验也非常有用。
### 适用范围
本书与 Django 3.x 系列配合良好，与 Django 2.2 等配合较差。尽管我们不保证功能上的兼容性，但至少本书大部分内容在 Django 1.0 之后的每个版本中都是适用的。

至于 Python 版本，本书在 Python 3.8 上进行了测试。大多数代码示例应该可以在Python 3.7.x或3.6.x上工作。但低于这个版本的应该就不行了。
### 每章内容相对独立
与每章都建立在前一章项目基础上的教程和演练书籍不同，我们在编写本书时有意让每章都相对独立。

我们这样做的目的是为了让你在工作中需要时，可以轻松参考有关特定主题的章节。

每章中的示例完全独立。它们并不打算合并成一个项目，也不是教程。请将它们视为有用的、独立的片段，以说明并帮助解决各种编码问题。
### 本书使用的约定
以下代码示例贯穿全书：
```python
class Scoop:
    def __init__(self):
        self._is_yummy = True
```
为了使这些代码段保持紧凑，我们有时会违反 PEP 8 中关于注释和行距的约定。代码示例在
<https://github.com/feldroy/two-scoops-of-django-3.x>

特殊的 "不要这样做！"代码块，如下面的代码块，表示您应该避免的不良代码示例
应避免的不良代码示例：
```python
Don’t!
class Scoop:
    def __init__(self):
        self._is_yummy = True
```
该段后面跳过未翻译
## 核心原则
### Keep It Simple, Stupid
在构建软件项目时，每一项不必要的复杂性都会增加添加新功能和维护旧功能的难度。增加新功能和维护旧功能的难度。尝试最简单的解决方案，但要注意不要实施过于简单的解决方案，以免做出错误的假设。这一概念简称为 "KISS"。
### Fat Models, Utility Modules, Thin Views, Stupid Templates
在决定将一段代码放在何处时，我们喜欢遵循 "胖模型、实用模块、瘦视图、笨模板 "的方法。我们建议，除了视图和模板外，不要在其他任何地方放置更多逻辑。这样做的结果是令人满意的。代码变得更清晰、更自文档化、更少重复，可重用性也大大提高。至于模板标记和过滤器，它们应该包含尽可能少的尽可能少的逻辑。
### 从Django的默认组件开始
在我们考虑将核心 Django 组件替换为其他模板引擎、不同的 ORM 或非关系型数据库之前，我们首先尝试使用标准 使用标准的 Django 组件。如果遇到障碍，再去尝试然后再替换 Django 核心组件。
### 熟悉 Django 的设计理念
定期阅读有关 Django设计理念的文档是件好事，因为它能帮助我们理解Django提供某些限制和工具的原因。与其他框架一样、Django不仅仅是一个提供视图的工具，它还是一种做事方式，旨在帮助我们在合理的时间内完成可维护的项目。
参考 <https://docs.djangoproject.com/en/3.2/misc/design-philosophies/>
### The Twelve-Factor App
十二要素应用程序是基于 Web 的应用程序设计的综合方法，在许多高级和核心 Django 开发人员中越来越受欢迎。它是一种构建可部署、可扩展应用程序的方法，值得阅读和理解。它的部分内容与本书中的实践非常吻合，我们喜欢将其视为将其视为任何基于 Web 的应用程序开发人员的建议读物。
## 我们的写作理念
在编写本书时，我们希望为读者和我们自己提供绝对最好的材料。为此，我们采用了以下原则：
### 提供最好的材料
我们尽最大努力提供最好的材料，对每个主题的已知资源来审查我们的材料。我们不怕提问！然后，我们将文章、回复和专家建议提炼成现在书中的内容。如果这些还不够，我们就提出自己的解决方案，并与各方面的专家进行了讨论。这是一项艰巨的工作，我们希望您能对结果感到满意。
如果您对本版本（Django 3.x）与上一版本（Django 1.11）之间的差异感到好奇，请参阅以下内容
版本（Django 1.11）的不同之处，您可以在以下链接查看
<https://github.com/feldroy/two-scoops-of-django-3.x/blob/master/changelog.md>
### 站在巨人的肩膀上
虽然我们对自己的工作表示肯定并承担责任，但本书中介绍的所有做法不完全是我们自己的实践。
如果没有构成 Django、Python 和通用开源软件社区的所有才华横溢、富有创造力和慷慨大方的开发人员，本书就不会存在。我们坚信，我们应该感谢那些曾经教过我们、指导过我们以及为我们提供信息的人，我们将尽最大努力给予他们应得的荣誉。我们已经尽了最大努力，在该归功的地方归功。
### 倾听读者和评论者的意见
在本书的前几版中，我们收到了大量的反馈意见，这些意见来自名副其实的读者和审稿人的大量反馈意见。这让我们大大提高了本书的质量。
现在，本书的质量达到了我们希望达到但从未达到的水平。
作为回报，我们在书后分享了我们的功劳，并将继续努力
通过改善世界各地开发人员的生活来回报社会。
此外，在书的末尾有一个链接，可以在亚马逊上为本书留言评论。这样做可以帮助其他人做出明智的决定，决定这本书是否适合他们。

该段后面跳过未翻译