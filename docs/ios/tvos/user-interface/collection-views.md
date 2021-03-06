---
title: "使用集合视图"
description: "本文介绍如何设计和集合视图在 Xamarin.tvOS 应用内部使用。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f943d6b88d2fd7f38759fb32ecb612e102266657
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-collection-views"></a>使用集合视图

_本文介绍如何设计和集合视图在 Xamarin.tvOS 应用内部使用。_

集合视图内容的一组允许要使用任意布局显示。 使用内置支持，它们允许对于轻松创建类似网格或线性布局，同时还支持自定义布局。

[![](collection-views-images/collection01.png "示例集合视图")](collection-views-images/collection01.png#lightbox)

集合视图维护使用委托和数据源提供用户交互和集合中的内容的项的集合。 由于集合视图基于独立于其自身的视图布局子系统，则提供一个不同的布局可以轻松更改表示法的集合视图的数据上快速。

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>有关集合视图

如上面集合视图中所述 (`UICollectionView`) 管理项的有序的集合，并为这些项显示的可自定义布局。 集合视图中的表视图类似的方式的工作 (`UITableView`)，但他们可以在远不止单个列中使用存在的项的布局。

如果在 tvOS 中使用集合视图，你的应用程序将负责提供与使用数据源的集合相关联的数据 (`UICollectionViewDataSource`)。 可以根据需要组织集合视图数据，并为其呈现到不同的组 （节）。

集合视图显示在屏幕上的单元格的各个项 (`UICollectionViewCell`)，它提供表示法的一段给定的集合 （如图像和其标题） 中的信息。

（可选） 可以向作为页眉和页脚的部分和单元格的集合视图的演示文稿添加补充视图。 集合视图的布局负责定义各个单元格以及这些视图的位置。

集合视图可以响应用户交互使用委托 (`UICollectionViewDelegate`)。 此委托程序还负责确定如果给定的单元格，如果突出显示单元格，也可能会得到焦点，或如果已选择了一个。 在某些情况下，委托确定各个单元格的大小。

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>集合视图布局

集合视图的一个主要功能是它所呈现的数据和其布局之间其隔离。 集合视图布局 (`UICollectionViewLayout`) 负责提供集合视图的屏幕上的演示文稿中具有组织和单元格 （以及任何补充视图） 的位置。

每个单元格中创建的集合视图从其附加的数据源和的然后排列和显示通过给定的集合视图布局。

创建集合视图时，通常被提供集合视图布局。 但是，你可以随时更改集合视图布局，自动将使用提供的新布局更新集合视图的数据在屏幕上显示。

集合视图布局提供了若干可用于进行动画处理 （默认情况下不完成任何动画） 的两个不同布局之间转换的方法。 此外，集合视图布局可以使用手势识别器要进一步进行动画处理的布局中的更改所导致的用户交互。

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>创建单元格和补充视图

集合视图的数据源不只负责提供用于显示内容的单元格的项，但是也支持集合的数据。

因为集合视图被设计为处理大量的项，则可以取消排队的各个单元格，并将其重复使用，以防止无限大内存限制。 有两个不同方法的取消排队视图：

- `DequeueReusableCell` -创建或返回 （在应用程序的情节提要中指定） 的给定类型的单元格。
- `DequeueReusableSupplementaryView` -创建或返回 （在应用程序的情节提要中指定） 的给定类型的补充视图。

在调用这些方法之一之前, 必须注册类，情节提要或`.xib`文件用于创建与集合视图的单元格的视图。 例如:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

其中`typeof(CityCollectionViewCell)`提供支持的视图的类和`CityViewDatasource.CardCellId`提供取消排队的单元格 （或视图） 时所使用的 ID。

取消排队单元格后，你将其配置与它所表示的项的数据，并返回到显示的集合视图。

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>有关集合视图控制器

集合视图控制器 (`UICollectionViewController`) 是专门的视图控制器 (`UIViewController`) 可提供以下行为：

- 它负责从其情节提要加载集合视图或`.xib`文件和实例化视图。 如果在代码中创建，它会自动创建新、 未配置的集合视图。
- 一旦加载集合视图后，控制器将尝试从情节提要加载其数据源和委托或`.xib`文件。 如果所有均不可用，它将自身设置为这两者的源。
- 确保数据之前集合视图填充第一天显示，都加载并重新加载和清除每个后续显示器上的选择。

此外，集合视图控制器提供了可重写方法可用来管理集合视图的生命周期如`AwakeFromNib`和`ViewWillDisplay`。

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>集合视图和情节提要

集合视图 Xamarin.tvOS 应用中使用的最简单方法是向其情节提要中添加一个。 作为一个快速示例，我们将创建的示例应用程序显示图像、 标题和选择按钮。 如果用户单击选择按钮，集合视图将显示允许用户选择新映像。 选择映像后，集合视图已关闭，并且将显示新的图像和标题。

让我们执行以下操作：

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. 启动新**单一视图 tvOS 应用**在 Visual Studio for mac。
1. 在**解决方案资源管理器**，双击`Main.storyboard`文件，并在 iOS 设计器中打开它。
1. 将图像视图、 标签和一个按钮添加到现有视图，并将它们配置为如下所示： 

    [![](collection-views-images/collection02.png "示例布局")](collection-views-images/collection02.png#lightbox)
1. 分配**名称**图像视图和中的标签**小组件选项卡**的**属性资源管理器**。 例如: 

    [![](collection-views-images/collection03.png "设置名")](collection-views-images/collection03.png#lightbox)
1. 接下来，拖动到情节提要的集合视图控制器： 

    [![](collection-views-images/collection04.png "集合视图控制器")](collection-views-images/collection04.png#lightbox)
1. 控制拖动按钮从到集合视图控制器，然后选择**推送**从弹出窗口： 

    [![](collection-views-images/collection05.png "从弹出窗口中选择推送")](collection-views-images/collection05.png#lightbox)
1. 当应用运行时，这将使用户单击按钮时，会显示集合视图。
1. 选择集合视图并输入中的以下值**布局选项卡**的**属性资源管理器**: 

    [![](collection-views-images/collection06.png "属性资源管理器")](collection-views-images/collection06.png#lightbox)
1. 此参数控制的大小以及各个单元格之间的单元格和集合视图的外边缘的边框。
1. 选择集合视图控制器并将其类设置为`CityCollectionViewController`中**小组件选项卡**: 

    [![](collection-views-images/collection07.png "将类设置为 CityCollectionViewController")](collection-views-images/collection07.png#lightbox)
1. 选择集合视图，并将其类设置为`CityCollectionView`中**小组件选项卡**: 

    [![](collection-views-images/collection08.png "将类设置为 CityCollectionView")](collection-views-images/collection08.png#lightbox)
1. 选择集合视图单元格并将其类设置为`CityCollectionViewCell`中**小组件选项卡**: 

    [![](collection-views-images/collection09.png "将类设置为 CityCollectionViewCell")](collection-views-images/collection09.png#lightbox)
1. 在**小组件选项卡**确保**布局**是`Flow`和**滚动方向**是`Vertical`集合视图： 

    [![](collection-views-images/collection10.png "小组件选项卡")](collection-views-images/collection10.png#lightbox)
1. 选择集合视图单元格并设置其**标识**到`CityCell`中**小组件选项卡**: 

    [![](collection-views-images/collection11.png "将标识设置为 CityCell")](collection-views-images/collection11.png#lightbox)
1. 保存更改。
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. 启动新**单一视图 tvOS 应用**Visual Studio 中。
1. 在**解决方案资源管理器**，双击`Main.storyboard`文件，并在 iOS 设计器中打开它。
1. 将图像视图、 标签和一个按钮添加到现有视图，并将它们配置为如下所示： 

    [![](collection-views-images/collection02vs.png "配置的布局")](collection-views-images/collection02vs.png#lightbox)
1. 分配**名称**图像视图和中的标签**小组件选项卡**的**属性资源管理器**。 例如: 

    [![](collection-views-images/collection03vs.png "属性资源管理器")](collection-views-images/collection03vs.png#lightbox)
1. 接下来，拖动到情节提要的集合视图控制器： 

    [![](collection-views-images/collection04vs.png "集合视图控制器")](collection-views-images/collection04vs.png#lightbox)
1. 控制拖动按钮从到集合视图控制器，然后选择**推送**从弹出窗口： 

    [![](collection-views-images/collection05vs.png "从弹出窗口中选择推送")](collection-views-images/collection05vs.png#lightbox)
1. 当应用运行时，这将使用户单击按钮时，会显示集合视图。
1. 选择集合视图并在**布局选项卡**的**属性资源管理器**输入**宽度**作为_361_和**高度**作为_256_ 
1. 此参数控制的大小以及各个单元格之间的单元格和集合视图的外边缘的边框。
1. 选择集合视图控制器并将其类设置为`CityCollectionViewController`中**小组件选项卡**: 

    [![](collection-views-images/collection07vs.png "将类设置为 CityCollectionViewController")](collection-views-images/collection07vs.png#lightbox)
1. 选择集合视图，并将其类设置为`CityCollectionView`中**小组件选项卡**: 

    [![](collection-views-images/collection08vs.png "将类设置为 CityCollectionView")](collection-views-images/collection08vs.png#lightbox)
1. 选择集合视图单元格并将其类设置为`CityCollectionViewCell`中**小组件选项卡**: 

    [![](collection-views-images/collection09vs.png "将类设置为 CityCollectionViewCell")](collection-views-images/collection09vs.png#lightbox)
1. 在**小组件选项卡**确保**布局**是`Flow`和**滚动方向**是`Vertical`集合视图： 

    [![](collection-views-images/collection10vs.png "Tthe 小组件选项卡")](collection-views-images/collection10vs.png#lightbox)
1. 选择集合视图单元格并设置其**标识**到`CityCell`中**小组件选项卡**: 

    [![](collection-views-images/collection11vs.png "将标识设置为 CityCell")](collection-views-images/collection11vs.png#lightbox)
1. 保存更改。
    

-----

如果我们已选`Custom`集合视图**布局**，我们无法指定自定义布局。 Apple 提供内置`UICollectionViewFlowLayout`和`UICollectionViewDelegateFlowLayout`，可以轻松地提供基于网格布局中的数据 (将使用这些`flow`布局样式)。 

有关使用情节提要的详细信息，请参阅我们[Hello，tvOS 快速入门指南](~/ios/tvos/get-started/hello-tvos.md)。

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>为集合视图提供数据

现在，我们已添加到我们的情节提要我们集合视图 （和集合视图控制器），我们需要找不到提供的数据。 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>数据模型

首先，我们将创建包含要显示的标题和允许市/县要选择的标志的图像的文件名我们数据的模型。

创建`CityInfo`类并使其如下所示：

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>集合视图单元格

现在，我们需要定义数据的每个单元中的显示方式。 编辑`CityCollectionViewCell.cs`（自动为你创建从您的情节提要文件） 的文件并使其如下所示：

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

我们将为我们 tvOS 的应用程序显示的映像和可选的标题。 如果不能选择给定的城市，我们变暗图像视图使用下面的代码：

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

包含图像的单元格用户使处于焦点时, 我们想要使用内置视差影响为设置以下属性：

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

有关导航和焦点的详细信息，请参阅我们[使用导航和焦点](~/ios/tvos/app-fundamentals/navigation-focus.md)和[Siri 远程和蓝牙控制器](~/ios/tvos/platform/remote-bluetooth.md)文档。


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>集合视图数据提供程序

使用我们创建的数据模型和定义我们单元格布局，让我们为我们集合视图创建一个数据源。 数据源将负责不仅提供备份数据，但还取消排队要显示在屏幕上的各个单元格的单元格。

创建`CityViewDatasource`类并使其如下所示：

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

允许查看详细信息中此类。 首先，我们继承自`UICollectionViewDataSource`和提供的单元格 id (iOS 设计器中，我们将分配） 的快捷方式：

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

接下来我们为我们集合的数据提供存储，并提供一个类，以填充数据：

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

然后我们重写`NumberOfSections`方法，并且我们集合视图的节 （项的组） 的数目已的返回。 在这种情况下，都只有一个：

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

接下来，我们将使用下面的代码我们集合中返回项的数目：

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

最后，我们取消排队的可重用单元格时集合视图请求替换为以下代码：

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

我们获得的集合视图单元格后我们`CityCollectionViewCell`类型，我们对其进行填充给定的项目。

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>响应用户事件

因为我们希望用户能够从集合中选择一个项，我们需要提供集合视图委托以处理此交互。 并且，我们需要提供方法，让我们知道用户的哪一项的调用视图已处于选中状态。

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>应用程序委托

我们需要一种方法使集合视图中的当前选定的项相关回调用视图。 我们将在使用自定义属性我们`AppDelegate`。 编辑`AppDelegate.cs`文件并添加以下代码：

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

这定义该属性和设置将最初显示默认城市。 更高版本，我们将使用以显示用户的选择，并允许选择要更改此属性。

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>集合视图委托

接下来，添加一个新`CityViewDelegate`类到项目并使其如下所示：


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

让我们仔细看一下此类。 首先，我们继承自`UICollectionViewDelegateFlowLayout`。 我们从此类继承的原因而不`UICollectionViewDelegate`是，我们将使用内置`UICollectionViewFlowLayout`能够提供我们项和不是自定义布局类型。

接下来，我们将返回使用此代码的各个项的大小：

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

然后，我们决定是否给定的单元格可以获得焦点使用下面的代码： 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

我们检查以查看一段给定的备份数据是否具有其`CanSelect`标志设置为`true`并返回该值。 有关导航和焦点的详细信息，请参阅我们[使用导航和焦点](~/ios/tvos/app-fundamentals/navigation-focus.md)和[Siri 远程和蓝牙控制器](~/ios/tvos/platform/remote-bluetooth.md)文档。

最后，我们响应用户选择某一项替换为以下代码：

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

此处我们设置`SelectedCity`属性我们`AppDelegate`到项，用户选择，我们关闭集合视图控制器，将返回到调用我们的视图。 我们尚未定义`ParentController`尚未我们集合视图的属性，我们将执行的下一步。

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>配置集合视图

现在，我们需要编辑我们集合视图，并分配我们的数据源和委托。 编辑`CityCollectionView.cs`（创建为我们自动从我们情节提要） 的文件并使其如下所示：

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

首先，我们提供快捷方式来访问我们`AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

接下来，我们将向集合视图的数据源和属性来访问集合的视图控制器 （使用上述我们委托来关闭集合，当用户进行的选择） 提供一种快捷方式：

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

然后，我们可以使用下面的代码来初始化集合视图并分配我们的单元格类，数据源和委托：

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

最后，我们想要仅会显示用户已将其突出显示的图像下方的标题 （焦点）。 我们实现替换为以下代码：

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

我们设置的失去焦点为零 (0) 的上一项 transparence 和下一项 transparence 获得焦点移到 100%。 这些例程都转换以及获取进行动画处理。


## <a name="configuring-the-collection-view-controller"></a>配置集合视图控制器

现在，我们需要执行我们集合视图上的最终配置，允许控制器设置我们定义这样的用户进行的选择之后，可以关闭集合视图的属性。

编辑`CityCollectionViewController.cs`（从自动创建我们情节提要） 的文件并使其如下所示：

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>综合程序 

现在，我们已经所有将放在一起的部分来填充和控制我们集合视图中，我们需要对我们的主视图将所有内容组合到一起进行最终的编辑。

编辑`ViewController.cs`（从自动创建我们情节提要） 的文件并使其如下所示：

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

下面的代码最初显示的选定的项`SelectedCity`属性`AppDelegate`和重新显示它时用户已从集合视图中进行选择：

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>测试应用程序

使用位置中的所有内容，如果生成并运行应用程序中的主视图将会显示与默认城市中：

[![](collection-views-images/run01.png "主屏幕")](collection-views-images/run01.png#lightbox)

如果用户单击**选择视图**按钮，集合视图才会显示：

[![](collection-views-images/run02.png "集合视图")](collection-views-images/run02.png#lightbox)

具有任何城市其`CanSelect`属性设置为`false`将显示灰色，而用户将不能将焦点设置到它。 当用户可以突出显示项目 (使其处于焦点) 显示标题和他们可以使用在三维种微妙倾斜图像视差效果。

当用户单击选择的映像时，集合视图已关闭，并且主视图将重新显示使用新的映像：

[![](collection-views-images/run03.png "主屏幕上新的映像")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>创建自定义布局和项重新排序

使用集合视图的主要功能之一是能够创建自定义布局。 由于从 iOS 继承 tvOS，用于创建自定义布局的过程是相同的。 请参阅我们[简介集合视图](~/ios/user-interface/controls/uicollectionview.md)文档以了解更多信息。

最近添加到集合视图适用于 iOS 9 时能够轻松地允许对集合中的项重新排序。 同样，由于 tvOS 9 的 iOS 9 子集，这完成这些相同的方式。 请参阅我们[集合视图更改](~/ios/user-interface/controls/uicollectionview.md)更多详细信息的文档。


<a name="Summary" />

## <a name="summary"></a>摘要

本文已覆盖设计和集合视图在 Xamarin.tvOS 应用内部使用。 首先，它讨论的所有元素构成集合视图。 接下来，它还介绍了如何设计和实现使用情节提要集合视图。 最后，为将提供有关创建自定义布局和项重新排序的信息的链接。



## <a name="related-links"></a>相关链接

- [tvOS 示例](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 人机接口指南](https://developer.apple.com/tvos/human-interface-guidelines/)
- [应用程序对 tvOS 的编程指南](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
