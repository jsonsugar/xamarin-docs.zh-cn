---
title: 环境
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c46d3aa151d4a596ca3da881a3c4e5e38bb41cbd
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2018
---
# <a name="environment"></a>环境

*执行环境*是一系列影响程序执行的环境变量。 可以在项目的属性中暂时设置环境变量，或通过将 mtouch 打包工具指定为额外参数来永久设置环境变量。

## <a name="temporary-environment-variables"></a>临时环境变量

临时环境变量在“运行”>“常规”部分中的项目“属性”/“选项”窗口中设置。 这些环境变量仅在使用 Visual Studio for Mac 启动应用程序时生效，如果通过在其上点击它手动启动应用，则不会设置环境变量。

## <a name="permanent-environment-variables"></a>永久环境变量

通过将 mtouch 打包工具指定为额外参数，设置永久环境变量。 这些环境变量被编译为可执行文件，即使应用不从 Visual Studio for Mac 中启动也会设置它们。

## <a name="example"></a>示例

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

