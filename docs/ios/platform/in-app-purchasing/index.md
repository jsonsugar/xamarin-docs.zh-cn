---
title: "应用内购买"
description: "iOS 应用程序可以销售数字产品和服务使用应用商店工具包 Api。 创建和管理在 iTunes Connect 门户产品。 Apple 管理事务处理和批准所有产品，然后他们可以销售，并收取每个事务 （当前 30%) 的费用。 Apple 要求你使用的应用程序，任何数字销售应用内购买，但不能将其用于销售的商品物理或非数字服务。 应用程序提供数字产品和服务的备用支付选项很可能被拒绝。 本文档介绍如何配置应用程序使用应用商店工具包，并提供的最常见的应用内购买情况的 Xamarin.iOS 示例。"
ms.topic: article
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: af8eb556215679bab2da8f54e8231f7d7d3ed418
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
---
# <a name="in-app-purchasing"></a>应用内购买

_iOS 应用程序可以销售数字产品和服务使用应用商店工具包 Api。创建和管理在 iTunes Connect 门户产品。Apple 管理事务处理和批准所有产品，然后他们可以销售，并收取每个事务 （当前 30%) 的费用。Apple 要求你使用的应用程序，任何数字销售应用内购买，但不能将其用于销售的商品物理或非数字服务。应用程序提供数字产品和服务的备用支付选项很可能被拒绝。本文档介绍如何配置应用程序使用应用商店工具包，并提供的最常见的应用内购买情况的 Xamarin.iOS 示例。_


iOS 应用程序可以销售数字产品或服务使用 StoreKit – 一组 Api 由与 Apple 的服务器的通信的 iOS 进行金融交易用户通过其 Apple id。 没有用户界面组件 – StoreKit Api 是主要关心检索产品信息和执行事务。 实现应用内购买的应用程序必须构建其自己的用户界面，并跟踪已购买的项替换为用于向用户提供的必需的产品或服务的自定义代码。

提供在应用内购买功能需要大量的步骤：

-  **配置你的应用**– 应用程序的预配配置文件，必须先设置正确。
-  **创建产品**– 产品说明和价格都必须在 iTunes Connect 门户中创建。
-  **实现 StoreKit** – 必须根据所售产品的类型实现了 StoreKit API。
-  **构建用户界面和产品本身**– 必须实现的产品，包括跟踪每个购买和备份/还原它们如果相应的机制。
-  **监视销售和接收资金**– 使用通过 iTunes Connect 提供的信息来监视销售趋势和跟踪你收入。


本文档说明如何完成所有这些步骤，以提供使用 Xamarin.iOS 的应用内购买。


## <a name="requirements"></a>惠?

若要支持的应用内购买必须使用 Xamarin.iOS 5.0 或更高版本，使用 Xcode 7 和更高版本。

## <a name="contents"></a>内容

 * [应用内购买基本知识和配置](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [存储工具包概述和检索产品信息](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [购买易耗型产品](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [购买非易耗型产品](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [事务和验证](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [订阅和报告](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)


## <a name="summary"></a>摘要

这篇文章了引入的应用内购买的概念、 概述如何配置应用程序以充分利用它，并且不提供使用 Xamarin.iOS 的示例。 已覆盖：

-  **iOS 设置门户**– 使应用程序中的准则购买功能。
-  **iTunes Connect** – 配置要销售你的应用程序中的产品。
-  **存储工具包**– 用于构建应用内购买功能的类的说明。
-  **编码的应用程序购买**– 如何在应用内购买融入 Xamarin.iOS 应用程序的示例。
-  **Reporting** – 通过 iTunes Connect 提供的统计信息的概述。


## <a name="related-links"></a>相关链接

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [在应用内购买编程指南](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect 开发人员指南](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [存储工具包 Framework 参考](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [应用内购买产品标识符问答](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [应用内购买技术说明](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [您的第一个应用商店提交](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [应用程序存储资源中心](https://developer.apple.com/appstore/index.html)
- [应用商店提交提示](https://developer.apple.com/appstore/resources/submission/tips.html)
- [应用商店查看准则](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [管理你的应用程序](https://developer.apple.com/appstore/resources/managing/index.html)
