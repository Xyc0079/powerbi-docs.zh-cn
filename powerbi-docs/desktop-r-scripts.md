---
title: 在 Power BI Desktop 中运行 R 脚本
description: 在 Power BI Desktop 中运行 R 脚本
author: davidiseminger
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-desktop
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: davidi
LocalizationGroup: Connect to data
ms.openlocfilehash: 6b69f701e0a5b9030a1f4469d6b09b189759debc
ms.sourcegitcommit: 64c860fcbf2969bf089cec358331a1fc1e0d39a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2019
ms.locfileid: "73876180"
---
# <a name="run-r-scripts-in-power-bi-desktop"></a>在 Power BI Desktop 中运行 R 脚本
可以直接在 **Power BI Desktop** 中运行 R 脚本并将所得数据集导入 Power BI Desktop 数据模型。

## <a name="install-r"></a>安装 R
若要在 Power BI Desktop 中运行 R 脚本，需要在本地计算机上安装 **R**。 可以从很多位置免费下载并安装 **R**，其中包括 [Revolution Open 下载页](https://mran.revolutionanalytics.com/download/)，以及 [CRAN 存储库](https://cran.r-project.org/bin/windows/base/)。 Power BI Desktop 中当前版本的 R 脚本在安装路径中支持 Unicode 字符以及空格（空字符）。

## <a name="run-r-scripts"></a>运行 R 脚本
在 Power BI Desktop 中只需几步即可运行 R 脚本并创建数据模型，从中可创建报表并在 Power BI 服务上共享它们。 Power BI Desktop 中的 R 脚本现在支持包含小数点 (.) 和逗号 (,) 的数字格式。

### <a name="prepare-an-r-script"></a>准备 R 脚本
若要在 Power BI Desktop 中运行 R 脚本，请在本地 R 开发环境中创建脚本并确保其已成功运行。

若要在 Power BI Desktop 中运行脚本，请确保该脚本可在未修改的新工作区中成功运行。 这意味着必须以显式方式加载和运行所有包和依赖项。 可以使用 *source()* 运行依赖脚本。

在 Power BI Desktop 中准备和运行 R 脚本时，会有一些限制：

* 仅会导入数据帧，因此请确保要导入到 Power BI 的数据都位于数据帧中
* 不导入类型为“复杂”和“向量”的列，且在创建的表中将其替代为错误值
* N/A 值将被转换为 Power BI Desktop 中的 NULL 值
* 任何 R 脚本若运行时间超过 30 分钟就会超时
* R 脚本中的交互式调用（例如等待用户输入）会终止脚本运行
* 在 R 脚本中设置工作目录时，*必须*定义工作目录的完整路径，而非相对路径

### <a name="run-your-r-script-and-import-data"></a>运行 R 脚本并导入数据
1. 在 Power BI Desktop 中，可在**获取数据**中找到 R 脚本数据连接器。 要运行 R 脚本，请选择“获取数据”&gt;“更多...”，然后选择“其他”&gt;“R 脚本”，如下图所示   ：
   
   ![](media/desktop-r-scripts/r-scripts-1.png)
2. 如果本地计算机上安装了 R，则会选择已安装的最新版本作为 R 引擎。 只需将脚本复制到脚本窗口，然后选择**确定**。
   
   ![](media/desktop-r-scripts/r-scripts-2.png)
3. 如果 R 尚未安装、无法识别，或者如果本地计算机上有多个安装，则展开 **R 安装设置**以显示安装选项或选择你想要用于运行 R 脚本的安装。
   
   ![](media/desktop-r-scripts/r-scripts-3.png)
   
   如果已安装 R 但无法识别，可在展开“R 安装设置”时提供的文本框中显式提供其位置  。 在上图中，已在文本框中以显式形式输入路径 *C:\Program Files\R\R-3.2.0*。
   
   R 安装设置集中位于“选项”对话框的 R 脚本部分。 要指定 R 安装设置，请选择“文件”和“选项和设置”，再依次选择“选项”和“R 脚本”   。 如果有多个 R 安装可用，则会显示一个下拉菜单，让你选择要使用的安装。
   
   ![](media/desktop-r-scripts/r-scripts-4.png)
4. 选择**确定**运行 R 脚本。 脚本成功运行后，即可选择要将其添加到 Power BI 模型的所得数据帧。

### <a name="refresh"></a>刷新
你可以在 Power BI Desktop 中刷新 R 脚本。 刷新 R 脚本时，Power BI Desktop 会再次在 Power BI Desktop 环境中运行 R 脚本。

## <a name="next-steps"></a>后续步骤
查看以下更多信息，了解有关 Power BI 中的 R。

* [在 Power BI Desktop 中创建 R 视觉对象](desktop-r-visuals.md)
* [将外部 R IDE 与 Power BI 一起使用](desktop-r-ide.md)

