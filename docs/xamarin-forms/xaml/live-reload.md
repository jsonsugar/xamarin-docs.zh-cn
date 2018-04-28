---
title: 实时重新加载
description: 请参阅对你的 XAML 的更改反映实时，而无需另一个编译和部署。
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 04/23/2018
ms.openlocfilehash: 11e876207285689b230bb2fada3a4c836443e360
ms.sourcegitcommit: a69439ad4c9fd0abe759143687d3b23582573d90
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="xamarin-live-reload"></a>Xamarin 实时重新加载

![预览](~/media/shared/preview.png)

Xamarin 实时重新加载使你能够**更改你的 XAML 和看到这些文件反映实时，而无需另一个编译和部署**。 将重新部署到你的 XAML 所做的任何更改将保存并反映在你的部署目标上。

因为使用实时重新加载时，已编译你的应用程序，它适用于所有库和第三方控件。 Xamarin.Forms 支持，包括 Android、 iOS 和 UWP，并且适用于所有有效的部署目标，包括模拟器、 仿真器和物理设备的所有平台上的实时重新加载工作原理。

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

实时重新加载目前仅在 Visual Studio 2017 中可用。

## <a name="requirements"></a>要求

* [Visual Studio 2017 15.7 Preview 4](https://www.visualstudio.com/vs/preview/)或更高版本与**使用.NET 进行移动开发**工作负荷。
* [Xamarin.Forms 3.0.354232-pre3](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3)或更高版本。

## <a name="getting-started"></a>入门
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1.从 Visual Studio 应用商店安装 Xamarin 实时重新加载。

通过 Visual Studio Marketplace 分发 Xamarin 实时重新加载。 若要安装扩展，请访问[Visual Studio 市场上的 Xamarin 实时重新加载页](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload)网站和单击**下载**。

打开.vsix，下载过程中，单击**安装**。

![Visual Studio 安装 Xamarin 实时重新加载确认](images/LiveReloadVSIXInstall.png)

> 或者，您可以在其中搜索中为其**联机**选项卡中**扩展和更新**在 Visual Studio 中的对话框。

### <a name="2-configure-your-app-to-use-live-reload"></a>2.配置你的应用程序以使用实时重新加载。

可以在三个步骤将实时重新加载添加到现有的移动应用程序：

1. 确保所有项目都更新为使用[Xamarin.Forms 3.0.354232-pre3](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3)或更高版本。
2. 安装**Xamarin.LiveReload**到你的.NET 标准 2.0 库的 NuGet。 这不需要安装在平台项目中。 确保**包源**设置为**所有**。

![添加 Xamarin 实时重新加载 NuGet 使用 NuGet 包管理器](images/addlivereloadnuget.png)

3. 添加`LiveReload.Init();`到中的构造函数`Application`类，如下面的代码段中所示：

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        LiveReload.Init();
    
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3.启动实时重新加载。

编译和部署应用程序。 部署应用程序后，打开 XAML 文件，进行一些更改，并保存文件。 所做的更改重新部署到部署目标。

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

实时重新加载适用于任何 XAML 文件的更改。 对 C# 或添加/删除 NuGet 包的更改要求新的生成和部署才会生效。

## <a name="frequently-asked-questions"></a>常见问题 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>是可在 Visual Studio 上适用于 Mac 的 Xamarin 实时重新加载？ 

Xamarin 实时重新加载的初始预览版本仅可用于 Visual Studio 2017。 Visual Studio 支持的适用于 Mac 计划为未来版本中。

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>此工作不与所有库，如 Prism？ 

由于你的应用被编译，实时重新加载适用于所有库，如 Prism 和第三方控件库，如 Telerik、 Infragistics、 Syncfusion、 ArcGIS、 GrapeCity 和其他控件供应商。

### <a name="what-changes-does-live-reload-redeploy"></a>实时重新加载重新部署哪些更改？ 

实时重新加载仅应用到使用 XAML 做的更改。 如果更改 C# 文件中，将需要重新编译。 重新加载 C# 支持计划为未来版本中。

### <a name="what-platforms-are-supported"></a>支持哪些平台？ 

通过 Xamarin.Forms，这包括 Android、 iOS 和 UWP 支持任何平台上的实时重新加载工作原理。

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>这是否适用于模拟器、 模拟器和物理设备？ 

是，实时重新加载适用于所有有效的部署目标，包括 Android 仿真程序、 iOS 模拟器和物理设备。 部署到设备要求的设备和计算机都位于相同的 Wi-fi 网络。

### <a name="does-this-work-with-corporate-networks"></a>这一点与公司网络呢？

如果你正在调试到 Android 仿真程序或 iOS 模拟器中，实时重新加载将使用 localhost 进行通信。 如果你想要部署到设备，设备和计算机需要在相同的 Wi-fi 网络。 你可以在其中无法做到这一点的情况下，[配置你自己的实时重新加载服务器](#live-reload-server)，可将让你为实时重新加载，而不考虑网络连接设置。

### <a name="does-it-require-debugging-the-app"></a>它是否需要调试应用程序？ 

不是。 事实上，甚至可以在任意数量的设备或模拟器/仿真程序上启动所有受支持的应用程序的目标 （Android、 iOS 和 UWP） 并将它们全部同时更新。 

## <a name="limitations"></a>限制

* 仅重新加载的 XAML 支持。
* 仅支持在 Visual Studio 中。
* 仅适用于.NET 标准库。
* 不支持 CSS 样式表。
* 除非使用 MVVM，则不可能将重新部署，之间保持 UI 状态。

## <a name="live-reload-server"></a>实时重新加载服务器

在方案中，从正在运行的应用到您的计算机的连接 (如由使用指示`localhost`或`127.0.0.1`中**工具 > 选项 > Xamarin > 实时重新加载**) 不能 （即防火墙、 不同的网络）你可以配置远程服务器相反，哪些 IDE 和应用程序将连接到。

实时重新加载使用标准[MQTT 协议](http://mqtt.org/)到交换消息，并因此可以与之通信[第三方服务器](https://github.com/mqtt/mqtt.github.io/wiki/servers)。 即使有[公共服务器](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers)(也称为*代理*) 可用，你可以使用。 实时重新加载经`broker.hivemq.com`和`iot.eclipse.org`主机名，以及提供的服务[www.cloudmqtt.com](https://www.cloudmqtt.com)和[www.cloudamqp.com](https://www.cloudamqp.com)。你还可以部署在云中，你自己 MQTT 服务器如[在 Azure 上的 HiveMQ](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud)或[上 AWS 兔子 MQ](http://www.rabbitmq.com/ec2.html)。 

你可以配置任何端口，但通常对远程服务器使用默认 1883年端口。 实时重新加载消息使用强端到端 AES 对称加密，以便安全地连接到远程服务器。 默认情况下，加密密钥和初始化向量 (IV) 重新生成对每个 Visual Studio 会话。

## <a name="troubleshooting"></a>疑难解答

当生成应用程序，从信息**工具 > 选项 > Xamarin > 实时重新加载**（主机名称、 端口和加密密钥） 嵌入在应用中，因此，当`LiveReload.Init();`运行时，没有配对或配置是若要成功执行了连接所必需。

非正常网络问题 （在不同网络上的设备中的防火墙），应用程序可能无法成功连接到 IDE 的主要原因是因为其配置不同于 Visual Studio 中的一个。 如果，则可能会发生这种情况：

* 编译应用程序时，在不同计算机上。
* 编译并在不同的 Visual Studio 会话中，部署应用程序没有找到和**自动生成加密密钥**是签入 （默认值）**工具 > 选项 > Xamarin > 实时重新加载**。
* Visual Studio 设置已更改 （即主机名、 端口或加密密钥），但未生成并再次部署应用程序。

这种情况下所有通过生成和重新部署应用程序解决。

## <a name="tips--tricks"></a>提示和技巧

* 只要不会更改的实时重新加载设置 (包括加密密钥，例如，如果你关闭**自动生成加密密钥**) 和中的同一台计算机生成，不需要生成并在初始后部署该应用程序部署，除非你更改代码或依赖项。 你可以只需重新启动之前部署的应用程序和它将到最后一个连接使用的主机。

* 可以连接到相同的 Visual Studio 会话的设备数量没有限制。 可以部署，还可以根据需要在同一时间对所有这些查看实时重装工作在尽可能多的设备模拟器中启动应用。

* 实时重新加载仅将重新加载您的应用程序的用户界面部分，但它不*不*重新创建页面，则两者不它替换你视图模型 （或绑定上下文）。 这意味着*整个*跨重新加载需要，包括插入依赖项将始终保留应用程序的状态。