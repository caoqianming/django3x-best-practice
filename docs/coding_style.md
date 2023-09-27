# 代码格式
稍加注意，遵循标准编码风格指南将大有裨益。我们强烈建议您阅读本章，尽管您可能想跳过它。
## 1.1 代码可读性很重要
程序必须是为人阅读而编写的，机器只是执行。

代码的可读性大于可写性。编写一个单独的代码块需要数分钟、调试需要几分钟或几小时，而且可能永远不会再被触及。当你或或其他人访问昨天或十年前编写的代码时，以清晰、一致的风格编写代码就变得非常有用。可理解的代码让人心情愉悦，不必再为不一致的地方费神，从而更容易维护和改进各种规模的项目。

这就意味着，你应该付出更多努力，使你的代码尽可能具有可读性。

1. 避免缩写变量名。
2. 写出函数参数名。
3. 记录你的类和方法。
4. 注释你的代码。
5.  将重复的代码行重构为可重用的函数或方法。
6. 保持函数和方法简短。一个好的经验法则是，在阅读整个函数或方法时不需要滚动

当你离开一段时间后再回到代码时，你会更容易从上次离开的地方重新开始。

以那些讨厌的缩写变量名为例。当你看到一个名为balance_sheet_decrease 的变量时，它比 bsd 或 bal_s_d 这样的缩写变量更容易在脑海中解释。这类偷懒方法可能会节省几秒钟的键入时间，但这些节省是以数个小时或数天的技术债务为代价的。这是不值得的。
## 1.2 PEP8
PEP 8 是 Python 的官方样式指南。我们建议详细阅读并学习遵循
PEP 8 编码约定：<https://python.org/dev/peps/pep-0008/>。
PEP 8 描述了编码规范，例如

1.  每个缩进级别使用 4 个空格
2.  "用两个空行分隔顶级函数和类定义
3.  类内的方法定义用一个空行隔开

Django 项目中的所有 Python 文件都应该遵循 PEP 8。 如果您记不住 PEP 8 准则，可以为您的代码编辑器找一个插件，在您输入代码时检查您的代码。

当一个有经验的 Python 程序员在一个 Django项目中存在严重违反 PEP 8 的行为时，即使他们什么也没说，也可能会往坏处想。相信我们这一点。

warning: PEP 8 的风格仅适用于新的 Django 项目。如果您的现有的 Django 项目遵循与 PEP 8 不同的约定，则遵循现有的约定。<https://python.org/dev/peps/pep-0008/#a-foolish-consistency-is-thehobgoblin-of-little-minds>

包推荐: Black用于python代码格式化，Flak8用于检查代码质量
<https://github.com/psf/black> <https://github.com/PyCQA/flake8>
### 1.2.1 79个字符的限制
根据 PEP 8，每行的文本限制为 79 个字符。这是因为它是一个安全值，大多数文本编辑器和开发团队都能适应，而不会影响代码的可理解性。
不过，PEP 8 也有一项规定，可以将这一限制放宽到 99 个字符，适用于专属的
团队项目。我们将其理解为非开源项目。
我们的偏好如下：

1. 在开源项目中，应硬性规定 79 个字符的限制。我们的经验表明，这些项目的贡献者或访问者会抱怨行长问题。但是，这并没有让贡献者离开，我们认为这并没有失去价值。

2. 在私人项目中，我们将限制放宽到 99 个字符，充分利用现代显示器的优势。
请阅读 <https://python.org/dev/peps/pep-0008/#maximum-line-leng>
## 1.3 关于Import
PEP 8 建议按以下顺序对导入进行分组：

1. 标准库导入
2. 相关的第三方导入
3. 本地应用程序或特定于库的导入

当我们在开发 Django 项目时，我们的导入会如下所示
```python
# Stdlib imports
from math import sqrt
from os.path import abspath

# Core Django imports
from django.db import models
from django.utils.translation import gettext_lazy as _

# Third-party app imports
from django_extensions.db.models import TimeStampedModel

# Imports from your apps
from splits.models import BananaSplit
```
Django 项目中的导入顺序是

1. 标准库导入。
2. 从 Django 核心导入。
3. 从第三方应用程序（包括与 Django 无关的应用程序）导入
4. 从作为 Django 项目一部分创建的应用程序导入
## 1.4 理解绝对/相对导入
在编写代码时，重要的是要让移动、重命名和编辑变得更容易和版本化工作。在 Python 中，显式相对导入是一种强大的方式，它可以将单个模块与它们周围的体系结构分离开来。因为Django 应用程序是简单的 Python 包，因此同样的规则也适用。为了说明显式相对导入的好处，让我们来看一个例子。想象一下以下代码段来自您创建的 Django 项目，该项目用于跟踪您的冰淇淋
消费情况，包括您吃过的所有华夫饼/糖/蛋糕筒。
```python
# cones/views.py
from django.views.generic import CreateView
# Relative imports of the 'cones' package
from .models import WaffleCone
from .forms import WaffleConeForm
# absolute import from the 'core' package
from core.views import FoodMixin
class WaffleConeCreateView(FoodMixin, CreateView):
model = WaffleCone
form_class = WaffleConeForm
```
当需要从当前app外导入时，使用绝对导入；当只是从本app其他模块导入时，使用相对导入
## 1.5 避免使用import *
这样做是为了避免隐式地将另一个 Python 模块的所有局部加载到和我们当前模块的命名空间，这会产生不可预知的，有时甚至是灾难性的结果。
### 1.5.1 一些Python的导入冲突
可以这样处理
```python
from django.db.models import CharField as ModelCharField
from django.forms import CharField as FormCharField
```
## 1.6 Django代码样式
本节既包括官方指南，也包括非官方但普遍接受的Django 约定。
### 1.6.1 考虑 Django 编码风格指南
不言而喻，了解常见的 Django 风格约定是个好主意。事实上，Django 内部有一套自己的风格指南，扩展了 PEP 8：
<https://docs.djangoproject.com/en/3.2/internals/contributing/writing-code/ coding-style/>
此外，虽然官方标准中没有指定以下内容，但它们在 Django 社区中足够常见，您可能会希望在您的项目中遵循它们。
<https://docs.djangoproject.com/en/3.2/internals/>
### 1.6.2 在 URL 中使用下划线而不是短横线
我们总是尽量使用下划线（"_"字符）而不是短横线。这不仅更Pythonic，而且对更多的集成开发环境和文本编辑器也更友好。请注意，这里我们指的是名称参数，而不是在浏览器中输入的实际 URL。
### 1.6.3 在模板块名称中使用下划线而不是短横线

tip: 部分跳过未翻译
