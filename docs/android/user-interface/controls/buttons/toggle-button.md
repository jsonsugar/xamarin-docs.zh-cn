---
title: ToggleButton
ms.topic: article
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 9e1e9711d218f4f4be825ff223b650ae932ad041
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
---
# <a name="togglebutton"></a>ToggleButton

在本部分中，你将创建专用于使用的两个状态之间切换按钮[ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)小组件。 此小组件是单选按钮的绝佳替代方法，如果你具有两个互相排斥的简单状态 ("打开"和"关闭"，例如)。 Android 4.0 （API 级别 14） 中引入了名为的切换按钮的替代方法[ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/)。

一个示例**ToggleButton**时映像的右对提供的示例所示的映像，左侧显示对**交换机**:

![在打开和关闭状态的开关和 ToggleButtons 示例](toggle-button-images/togglebutton-switch.png)  

应用程序使用哪个控件是一种样式。 这两个小组件在功能上等效。

打开**Resources/layout/Main.axml**文件并添加[ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)元素 (内[ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

若要在状态更改时执行某些操作，请将下面的代码添加到末尾的[ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)方法：

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

这将捕获[ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)元素从布局中，并处理 Click 事件，定义要在单击该按钮时执行的操作。 在此示例中，该方法检查此按钮时，新状态，然后显示[ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)消息，指出当前的状态。

请注意， [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)其自己的状态更改之间 checked 和 unchecked，以使你只需要求它本来就是句柄。

运行该应用程序。


**提示：**如果你需要自行更改的状态 (例如，当加载已保存[ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/))，使用[ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)属性 setter 或[ `Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)方法。


## <a name="related-links"></a>相关链接

- [ToggleButton](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [交换机](http://developer.android.com/reference/android/widget/Switch.html)
