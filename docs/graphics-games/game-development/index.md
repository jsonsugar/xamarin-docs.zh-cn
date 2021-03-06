---
title: "游戏开发使用 Xamarin 简介"
description: "游戏开发的性质可能相差很从其他类型的应用的开发。 本文是具有可以用于 Xamarin.iOS 和 Xamarin.Android 的技术的游戏开发的简介。 它使用 Xamarin.iOS 和 Xamarin.Android 提供如何进行游戏的高级别讨论以及技术可供使用的采样。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 9d1ce2da87d6f169efb5431f734695f6876cf3f0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-xamarin"></a>游戏开发使用 Xamarin 简介

_游戏开发的性质可能相差很从其他类型的应用的开发。本文是具有可以用于 Xamarin.iOS 和 Xamarin.Android 的技术的游戏开发的简介。它使用 Xamarin.iOS 和 Xamarin.Android 提供如何进行游戏的高级别讨论以及技术可供使用的采样。_

开发游戏可以是非常令人兴奋，尤其是考虑如何轻松就越大，发布你的移动平台上工作。 本文讨论了概念和技术与将帮助你的游戏开发创建游戏，是否你的目标是要创建高质量 AAA 游戏或者只是为了更加有趣的程序。

本文介绍了以下主题：

- **与非游戏编程概念游戏**– 我们将探讨都是一些概念特有游戏开发，或与其他类型的开发共享，但值得强调此处由于它们的重要性。
- **游戏开发团队**– 本部分介绍中一组游戏开发人员的各种角色。
- **创建一个游戏主意**– 这一部分可以帮助你创建一个新的游戏主意 – 进行了新游戏的第一步。
- **游戏开发技术**– 此处我们将列出了一些的跨平台可用的技术，这样可以提高作为游戏开发人员工作效率。


# <a name="game-vs-non-game-programming-concepts"></a>游戏 vs。非游戏编程概念

将移动到游戏开发的程序员通常面对新概念和开发模式。 本节介绍其中一些概念的高级视图。


## <a name="the-game-loop"></a>游戏循环

典型的游戏需要常量移动或更改发生在以响应用户交互和自动游戏逻辑屏幕上。 这将通过什么通常称为实现*游戏循环*。 游戏循环是某种类型的循环语句 （如 while 循环） 用于运行在非常高频率，例如 30 或 60*每秒帧数*。

以下是一个简单的游戏循环的图表：

![](images/image1.png "这是简单的游戏循环的关系图")

下面我们将讨论的技术将抽象化实际的 while 循环，但这种抽象形式尽管每帧更新的概念将会显示。

代码性能可以获得中甚至最简单的游戏的优先级。 例如： 采用 10 毫秒，若要执行的函数无法对的游戏 – 性能有显著的影响尤其是如果它调用则有不止一次每帧。 如果你的游戏运行在每 30 帧第二个则表示每个帧必须执行下 33 （毫秒）。 与此相反，此类函数可能甚至不明显如果它仅在响应中非游戏应用程序的按钮单击中执行。

可能执行的每个帧的逻辑的常见类型包括：

- **读取输入**– 游戏可能需要检查是否用户具有与之交互游戏通过检查输入的硬件，如触摸屏、 键盘、 鼠标或游戏控制器。
- **移动**– 对象从一个位置的移动到另一个通常将很小金额来虚构出流畅的每个帧。
- **冲突**– 很多游戏需要频繁的测试的各种对象是否是重叠，或者交叉。 我们将介绍这篇文章中后面一节中更深入的冲突。 可能由专用的物理模拟系统处理移动和冲突。
- **检查游戏特定条件**– 可能由某些情况下，控制的游戏的状态，如是否玩家获得足够的点或是否已分配的时间用完。
- **AI 行为**– 可用于控制对象不受玩家的如的防范对象或解决椭圆形对手驱动程序的移动 patrolling 的行为的每个帧逻辑。
- **呈现**– 大多数游戏将更新显示的内容在屏幕的每一帧。 这可能为了响应更改的影响 （例如级别范围内移动一个字符） 的游戏或只需提供 visual 波兰语 （如下降 snow 或动画的图标）。

记住许多上面列出的活动可以更改的状态的整个应用程序，而许多非游戏应用程序往往会改变以引发的事件响应的状态。


## <a name="content-loading-and-unloading"></a>内容的加载和卸载

具体取决于哪种技术开发中使用，可能需要手动加载和卸载 （或释放） 的内容。 手动加载和卸载的资产可能出于多种原因导致：

 - 资产可能需要很长时间才能加载相对于单个帧的长度。 某些资产甚至可能使秒钟以加载，如果加载 mid 玩游戏严重会干扰体验。 如果加载时间，尤其是长时间 （如多个第二个或两个） 你可能想要显示的动态加载屏幕或进度栏。
 - 资产可以消耗大量的 RAM，需活动管理的新增功能加载以适合提供的游戏的目标平台的内容。
 - 游戏可能需要显示多个资产比适合在 RAM。 "打开 World"游戏通常包括大型环境中哪些播放器可以无缝地导航 – 这是与没有加载屏幕。 在这种情况下你可能需要创建自定义系统中的流式处理内容和管理内存使用情况。

自定义文件格式可能需要在加载时的处理需要自定义加载代码。


## <a name="math"></a>数学

很多游戏需要更高级的数学比非游戏应用程序。 当然，数学程度取决于游戏的复杂性。 一般情况下 3D 游戏需要多个数学比二维。 幸运的是你可以始终使用简单的游戏和入门了解根据自己的意愿。 游戏开发可以是一个好办法了解数学 ！

如果你熟悉笛卡尔平面 – 使用 X 和 Y 坐标位置对象 – 然后知道足以游戏开发入门。 下面演示的笛卡尔平面与正 Y 指向上方：

![](images/image2.png "下面的示例演示笛卡尔平面与正 Y 指向向上")

> [!IMPORTANT]
> 一些引擎/Api 使用坐标系统，其中增加对象的 Y 值将向下移动，而其他系统使用正 Y 由一个坐标系统。 如果你是系统之间移动，请记住这一点。
三角函数 （如正弦值和余弦值） 通常用于实现任何形式的旋转二维游戏。



如果你打算在进行 3D 游戏然后可能将需要熟悉线性代数 （适用于在三维空间中的移动和旋转） 以及某些微积分 （用于实现加速） 的概念。


## <a name="content-pipelines"></a>内容的管道

术语*内容的管道*指到其最终格式时使用游戏中的文件从其格式 （如.png 图像文件） 创作时获取所需的过程。 结束的格式取决于在其上的内容类型正在使用正在用于技术以及其呈现内容。

某些内容的管道可能是非常快，并且要求任何手动干预。 例如，大多数游戏引擎和 Api 可以加载其未处理的格式中的.png 文件格式。 另一方面，更复杂的格式 （例如 3D 模型） 可能需要在正在加载之前要处理为其他格式，并且此处理可能需要一些时间，具体取决于资产的大小和复杂性。


# <a name="game-development-teams"></a>游戏开发团队

游戏开发过程中涉及的个人引入了新的角色和标题。 大多数游戏开发人员不能满足广泛的所需释放完整的游戏，因此存在大量的专业技能。 请记住，这不开发 – 更常见的一些区域的完整列表。

- **程序员**– 大多数人阅读这篇文章都属于此类别。 程序员游戏开发中的角色是类似于在非游戏应用程序的程序员的角色。 其职责包括编写逻辑控制流的游戏，开发的上下文中的常见任务的给定的项目，添加和显示内容，以及 – 当然 – 修复 bug 的系统。
- **二维艺术家**– 二维艺术家负责创建*二维资产*。 这些包括游戏的 GUI、 粒子、 环境和字符图像文件。 如果你正在开发的游戏三维，则二维艺术家可能不能负责环境和字符。 你可以找到免费为在你的游戏的艺术作品[http://opengameart.org/](http://opengameart.org/) 。
- **3D 艺术家**– 3D 艺术家负责创建*三维资产*。 其中包括用于环境、 字符和属性 （设备、 工厂和其他非动画对象） 的三维模型。 一些团队了解 3D 专业人员和三维动画制作人员，具体取决于团队的大小之间的区别。 你可以找到免费为在你的游戏的 3D 画[http://opengameart.org/](http://opengameart.org/) 。
- **游戏设计器**– 游戏的设计器负责定义如何播放游戏。 这可能包括高级决策例如的设置的游戏、 游戏，和如何播放器进程游戏中的总体目标。 定义对于移动或级别的麻烦，系数和设计级别布局，则也可以非常详细的决定，例如输入映射到操作中涉及游戏的设计器。 请记住，术语*设计器*可能引用游戏的设计器或可视化的设计器中，具体取决于上下文。
- **声音设计器**– 声音的设计器负责游戏的音频资产。 一些团队可能区分个人负责创建声音效果和作曲者，而较小的团队可能有一个人负责所有音频。


# <a name="creating-a-game-idea"></a>创建一个游戏的想法

设计游戏可能会出现很简单的操作 – 在所有的唯一要求是"使有趣的内容。" 遗憾的是，许多开发人员会发现自己在丢失时若要创建要启动开发从中了解。

游戏设计的专业未轻松地解释，，所需做法提高一样艺术作品或编程存在，但这一部分可以帮助你开始路径下。

新的开发人员应首先小。 它可能很难反对以重新创建大型、 现代视频游戏的倾向做法，但较小的游戏可以更好的学习环境并可更快的进度使人满意的体验。

很多游戏，同时用于学习以及商业游戏，开始以改进或修改现有的游戏。 生成的想法的一种方法是在灵感其他游戏查看。 例如，你可以考虑您个人喜欢的游戏，然后重试以了解有关玩游戏哪些特征更加有趣。 它可能是浏览，精通游戏所需的机制，或通过某个情景的进展情况。 不要忘记时要考虑"怀旧式"游戏以及搜索新的想法。

用于生成新的想法的另一种方法是考虑特定流派，如拼图游戏、 策略游戏或 platformers。 为开发人员熟悉一种风格可能会提供很好的起点。

重新现有游戏也是教育的体验，虽然这可能会限制完成的产品的商业生存能力。 创建一个游戏，即使其中一个是准确的克隆的过程提供有价值的教育体验。


# <a name="game-development-technology"></a>游戏开发技术

开发人员使用 Xamarin.Android 和 Xamarin.iOS 有大量技术可用于帮助游戏开发。 本部分将讨论一些最受欢迎的跨平台解决方案。


## <a name="cocossharp"></a>CocosSharp

CocosSharp 是科科斯二维游戏引擎的开源的跨平台版本。 引擎提供对 Android、 iOS、 Mac OS X、 Windows 桌面、 Windows RT 和 Windows Phone 访问。

CocosSharp 着重于二维游戏开发简单程序员 API。 在移动设备上的游戏中的增长有助于 reignite 进行 CocosSharp 可行技术，用于爱好和商业项目相似的二维游戏开发的受欢迎程度。 它提供的源的代码或.dll 文件 （这可通过 NuGet 获取），但它不提供的可视编辑器;因此，与 CocosSharp 引擎的任何交互需要的知识。

若要开始使用 CocosSharp，请查看我们[CocosSharp 指南](~/graphics-games/cocossharp/index.md)。

游戏使用 CocosSharp，创建生气 Ninjas，并且如果你正在寻找多个平台一个已在运行的游戏，可以很好的起点：

![](images/image3.png "游戏使用 CocosSharp 创建生气 Ninjas")

你可以下载它并获取详细信息在[AngryNinjas Github 页面](https://github.com/xamarin/AngryNinjas)。


## <a name="monogame"></a>MonoGame

MonoGame 是开源跨平台版本的 Microsoft XNA API。 MonoGame 可用来制作为 iOS、 Android、 Mac OS X、 Linux、 Windows、 Windows RT 和 Windows Phone 的游戏。

不同于 CocosSharp，MonoGame 才从技术上讲不游戏引擎，但而是一个游戏开发 API。 这意味着使用 MonoGame 需要直接管理游戏对象、 手动绘制对象，并实现常见对象，如摄像头和*场景关系图*（父子层次结构之间游戏对象）。 为了帮助了解它们的区别，请考虑 CocosSharp 基于 MonoGame。 MonoGame 通用化一些特定于平台的技术，如图形、 呈现和音频，而 CocosSharp 提供用于组织和实现游戏逻辑的代码。

MonoGame 不提供标准 visual 开发环境中，因此使用 MonoGame 要求编程知识。

使用 MonoGame 的游戏的值得注意的示例包括：

FEZ:

![](images/image7.png "FEZ")

堡垒：

![](images/image8.jpg "Bastion")

若要开始使用 MonoGame，转到我们[MonoGame 指南](~/graphics-games/monogame/index.md)。


## <a name="urhosharp"></a>UrhoSharp

UrhoSharp 是一个跨平台高级 3D 和 2D 引擎，可用来创建动画 3D 和二维场景为应用程序使用几何图形、 材料、 灯和照相机。

![](images/urhosharp.gif "UrhoSharp 是一个跨平台高级 3D 和 2D 引擎，可用来创建动画的 3D 和 2D 场景")

签出[UrhoSharp 指南](~/graphics-games/urhosharp/index.md)吧。

## <a name="additional-technology"></a>其他技术

上面突出显示的技术是仅一个可用的技术的示例。 其他值得注意的技术包括：

- **动画层工具包**– Xamarin 提供了对 Apple 的画面工具包游戏框架，它使你可以访问所有本机 API 的功能的支持。 由于动画层工具包是由 Apple 创建技术，它提供了与 iOS 生态系统的其余部分的深度集成。 当然，画面工具包是不可跨平台，因此它无法在 Android 上使用。 使用动画层工具包的详细信息，请参阅此文章： [http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **场景工具包**– Xamarin 还提供对 Apple 的场景工具包框架，它简化了到 iOS 应用程序中实现 3D 图形的支持。 场景工具包也是 Apple，因此它具有的集成和特定于平台的注意事项上述画面工具包提供的技术。 场景工具包的详细信息，请参阅此文章： [http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **OpenTK –** OpenTK （它代表打开工具包） 提供低级别的 OpenGL 访问，iOS、 Apple 和 Mac 硬件。 OpenTK 的详细信息，请参阅在主页： [http://www.opentk.com/](http://www.opentk.com/)


# <a name="summary"></a>摘要

本文介绍游戏开发的主要概念，并提供有关如何开始进行第一个游戏的信息。 完成这篇文章后下, 一步的步骤是选择您的技术，并开始通过我们系列教程在前面的相应章节中链接的工作。

## <a name="related-links"></a>相关链接

- [CocosSharp 指南](~/graphics-games/cocossharp/index.md)
- [MonoGame 指南](~/graphics-games/monogame/index.md)
- [UrhoSharp 指南](~/graphics-games/urhosharp/index.md)
