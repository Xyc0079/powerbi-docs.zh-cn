---
title: 使用 Power BI Desktop 中的“问答”功能
description: 现在可以使用 Power BI Desktop 中的“问答”功能进行自然语言查询
author: maggiesMSFT
manager: kfile
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-desktop
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: maggies
LocalizationGroup: Create reports
ms.openlocfilehash: a34de25af7febf63b34d7608ff7ef40e8b653fc4
ms.sourcegitcommit: 60dad5aa0d85db790553e537bf8ac34ee3289ba3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "61284654"
---
# <a name="use-qa-in-power-bi-desktop-for-natural-language-queries"></a>使用 Power BI Desktop 中的“问答”功能进行自然语言查询
使用常用短语和自然语言来查询数据是一个非常有用的功能。 而数据在回答问题时的功能更为强大，这就是 **Power BI Desktop** 中的“问答”功能所实现的。

为了能够让“问答”功能成功地解释其能够进行回答的大型问题集合，“问答”功能就必须对有关模型进行假设。 如果你的模型结构不能满足一个或多个这些假设，则需要对其进行调整。 针对“问答”功能的这些调整，对于 Power BI 中的任何模型来说都是同样的最佳做法优化，无论你是否使用“问答”。 

> [!NOTE]
> 仅在使用的模型包含已导入的数据，或者要实时连接到 SSAS 时，才可使用“问答”功能。 不支持 DirectQuery 模型。
>
> 如果使用 Windows 10 之前的 Windows 版本，问答需要以下 C 运行时更新。 可以尝试通过 Windows 更新安装重要更新，也可以尝试通过 Microsoft (KB2999226) 手动安装必需组件。 [https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows)
>
>

在以下部分中，我们将介绍如何调整模型，以使其能够很好地适用于 Power BI 中的“问答”功能。

## <a name="add-missing-relationships"></a>添加缺少的关系

如果模型缺少表之间的关系，则在你提问有关表的问题时，Power BI 报表和“问答”功能都将无法解释如何联接这些表。 关系是一个优秀模型的基石。 例如，如果“订单”表和“客户”表之间缺少关系，则无法提问类似“西雅图客户的销售总额”的问题。 下图显示了一个需要进行调整的模型示例，以及一个可用于“问答”的模型。

**需要调整**

![需要针对“问答”调整的关系](media/desktop-qna-in-reports/desktop-qna_01.png)

**可用于“问答”**

![已调整好可用于“问答”的关系](media/desktop-qna-in-reports/desktop-qna_02.png)


## <a name="rename-tables-and-columns"></a>重命名表和列

表和列的选择对“问答”非常重要。 例如，假设你有一个名为“客户汇总”的表，其中包含你的客户列表。 你需要提问类似“列出芝加哥的客户汇总”的问题，而不是提问“列出芝加哥的客户”。 

尽管“问答”功能可以执行一些基本的断词和复数检测，但其将假定你的表和列名能够准确地反映自身包含的内容。

请看另一个示例。 假设你有一个名为“员工总数”的表，其中包含员工的姓氏、名字和编号。 你还有另外一个名为“员工”的表，其中包含员工编号、作业编号和开始日期。 熟悉此模型的人可能会理解此结构。 其他人提问“员工计数”时，将获得“员工”表中的行的总数。 此结果可能不是他们想要的结果，因为它是每个员工曾经拥有的每一项作业的总数。 因此，最好重命名这些表，以真正反映它们所包含的信息。

**需要调整**

![需要针对“问答”调整的表名称](media/desktop-qna-in-reports/desktop-qna_03.png)

**可用于“问答”**

![已调整好的可用于“问答”的表](media/desktop-qna-in-reports/desktop-qna_04.png)

## <a name="fix-incorrect-data-types"></a>修复不正确的数据类型

导入的数据可能包含错误的数据类型。 具体而言，以“字符串”导入的“日期”和“编号”列将不会被“问答”功能解释为日期和编号。 请确保在 Power BI 模型中选择正确的数据类型。

![选择正确的数据类型，以确保可将其用于“问答”。](media/desktop-qna-in-reports/desktop-qna_05.png)

## <a name="mark-year-and-identifier-columns-as-dont-summarize"></a>将“年份”和“标识符”列标记为“不汇总”

在默认情况下，Power BI 将主动聚合数值列，因此，“年销售总额”等提问的结果有时可能会是多年度总计的总销售额。 如果有特定列不希望让 Power BI 进行汇总，则可以将列的“汇总方式”属性设置为“不汇总”。 请注意“年”、“月”、“日”和 ID 列，因为这些列最容易出现问题。 对不适合汇总的其他列（如年龄），将“汇总方式”设置为“不汇总”或“平均值”，也可以从中受益。 可以在“建模”选项卡中找到此设置。

![不为“问答”汇总年、月、日期等列](media/desktop-qna-in-reports/desktop-qna_06.png)

## <a name="choose-a-data-category-for-each-date-and-geography-column"></a>为每个日期和地理列选择数据类别

“数据类别”可提供有关列数据类型以外的列内容的语义知识。 例如，整数列可能被标记为邮政编码，字符串列可能被标记为城市、国家/地区、区域等。 “问答”功能将该信息用于两个重要方面：用于可视化效果选择和用于语言偏好设置。

首先，“问答”使用数据类别信息帮助选择要使用什么类型的视觉显示。 例如，它可以识别出具有日期或时间数据类别的列，这对于使用水平轴的折线图或播放轴的气泡图来说都是一个不错的选择。 并且，它还假设包含具有地理数据类别的列的结果在地图上显示效果良好。

其次，“问答”功能将对用户可能讨论的日期和地理列进行一些推理，以帮助其理解某些类型的问题。 例如“何时雇用的 John Smith？”中的“何时” 几乎能够肯定映射到日期列，而“Brown 的客户计数”中的“Brown”更有可能是一个城市，而不是头发的颜色。


![为“问答”选择适当的数据类别](media/desktop-qna-in-reports/desktop-qna_07.png)

## <a name="choose-a-sort-by-column-for-relevant-columns"></a>为相关列选择“按列排序”

使用“按列排序”属性，可以对一个列进行排序，而不是自动按其他列进行排序。 例如，当提问“按衬衫尺寸对客户排序”时，你可能希望你的“衬衫尺寸”列按基础尺寸编号（XS、S、M、L、XL）排序，而不是按字母顺序（L、M、S、XL、XS）排序。

![为“问答”选择适当的“按列排序”](media/desktop-qna-in-reports/desktop-qna_08.png)

## <a name="normalize-your-model"></a>规范化你的模型

请放心，我们并不是建议你需要重新构建整个模型。 但是，一些结构难度很大，“问答”功能无法很好地对其进行处理。 如果对模型的结构执行一些基本的规范化操作，则 Power BI 报表的可用性将会得到显著提升，“问答”结果也会随之更为准确。

请遵循此一般规则：用户讨论的每个唯一“事项”都应由相应的一个模型对象（表或列）来表示。 因此，如果你的用户讨论客户，则应有一个 customer 对象。 如果你的用户讨论销售额，则应有一个 sales 对象。 听起来很简单，对吗？ 取决于你开始使用的数据形状，它可以很简单。 如果需要，查询编辑器中提供了丰富的数据构形功能，而使用 Power BI 模型中的计算能够更为直接的进行转换。

以下部分包含可能需要执行的一些常见转换。

### <a name="create-new-tables-for-multi-column-entities"></a>为多列实体创建新表

如果有多个列充当较大表内的单个不同单元，则应将这些列拆分为其自己的表。 例如，假设你的“公司”表中包含“联系人姓名”、“联系人职务”和“联系人电话”列。 则最好设计一个单独的“联系人”表，以包含姓名、职务和电话，以及一个指回公司表的链接。 与在公司表中提问谁是联系人相比，单独就联系人表进行提问要简单很多，且提高了显示的灵活性。

**需要调整**

![为“问答”使用多个表](media/desktop-qna-in-reports/desktop-qna_09.png)

**可用于“问答”**

![为“问答”使用多个表](media/desktop-qna-in-reports/desktop-qna_10.png)

### <a name="pivot-to-eliminate-property-bags"></a>消除属性包的透视

如果在模型中存在属性包，则应对其重新构建以使每个属性都具有单个列。 尽管属性包便于管理大量的属性，但依然存在许多固有限制，Power BI 报表和问答功能都没有解决此问题的方法。

例如，考虑采用一个 CustomerDemographics 表，用以包含客户 ID、属性和值列，其中每行表示客户的一个不同属性（例如，年龄、婚姻状况、城市等）。 通过基于属性列的内容重载值列的含义，“问答”功能无法解释引用它的大部分查询。 简单的问题（如“显示每个客户的年龄”）可能碰巧起作用，因为它可能被解释为“显示属性为年龄的客户和客户人口统计数据”。 但是，模型结构将无法支持稍微复杂一点的问题，如“芝加哥客户的平均年龄”。 尽管直接创作 Power BI 报表的用户有时可找到更聪明的方法来获取他们正在寻找的数据，但只有当每列仅有一个含义时，问答才起作用。

**需要调整**

![不要在模型中为“问答”使用属性包](media/desktop-qna-in-reports/desktop-qna_11.png)

**可用于“问答”**

![为“问答”使用多个表](media/desktop-qna-in-reports/desktop-qna_12.png)

### <a name="union-to-eliminate-partitioning"></a>消除分区的联合

如果已跨多个表对数据进行了分区，或拥有跨多个列的透视值，则你的用户将很难或无法实现许多常用的操作。 请首先考虑典型表分区：一个“销售额 2000-2010”表和一个“销售额 2011-2020”表。 如果你的所有重要报表都被限制为特定的十年，则可以为 Power BI 报表保留这种方式。 但是，问答的灵活性会让你的用户期望得到“按年销售总额”等问题的答案。 若要执行此查询，需要将数据联合到单个 Power BI 模型表中。

同样的，请考虑采用典型的透视值列：一个包含作者、书籍、城市 1、城市 2 和城市 3 列的 BookTour 表。 对于此类结构，即使一个简单的问题（如“按城市计数书籍”）都无法正确解释。 若要执行此查询，请创建一个单独的 BookTourCities 表，将城市值联合到单个列。

**需要调整**

![不要在模型中为“问答”使用属性包](media/desktop-qna-in-reports/desktop-qna_13.png)

**可用于“问答”**

![为“问答”使用多个表](media/desktop-qna-in-reports/desktop-qna_14.png)

### <a name="split-formatted-columns"></a>拆分格式化列

如果导入数据的源包含格式化的列，则 Power BI 报表（和问答）将无法访问列内部以分析其内容。 因此，例如，如果有一个包含地址、城市和国家/地区的“完整地址”列，还应将其拆分为地址、城市和国家/地区列，以便你的用户能够单独对各列进行查询。

**需要调整**

![不要为“问答”使用多个数据元素的单个列](media/desktop-qna-in-reports/desktop-qna_15.png)

**可用于“问答”**

![为“问答”使用多个表](media/desktop-qna-in-reports/desktop-qna_16.png)

同样的，如果你有某人的任何完整姓名列，则需要添加名字和姓氏列，以防可能有人会使用姓名的一部分进行提问。 


### <a name="create-new-tables-for-multi-value-columns"></a>为多值列创建新表

情况类似，如果导入数据的源包含多值列，则 Power BI 报表（和问答）将无法访问列内部以分析其内容。 因此，例如，如果有一个“作曲者”列，包含有一首歌曲的多个作曲者姓名，则应在单独的“作曲者”表中将其拆分为多个行。

**需要调整**

![不要为“问答”使用多值列](media/desktop-qna-in-reports/desktop-qna_17.png)

**可用于“问答”**

![为“问答”使用多个表](media/desktop-qna-in-reports/desktop-qna_18.png)

### <a name="denormalize-to-eliminate-inactive-relationships"></a>规范化以消除非活动关系

“规范化更好”规则的一个例外是当存在从一个表到另一个表的多个路径时。 例如，如果有一个“航班”表，包含出发城市 ID 和目标城市 ID 列，其中每列都与“城市”表相关。 需将其中的一个关系标记为非活动关系。 因为“问答”只能使用活动关系，所以无法提问有关出发城市或目标城市的问题，具体要取决于所选的关系是什么。 如果改为使城市名称列非规范化为“航班”表，则可以提问类似“列出明天包含西雅图出发城市和旧金山目标城市的航班”等的问题。

**需要调整**

![为“问答”的每个表仅使用一条路径](media/desktop-qna-in-reports/desktop-qna_19.png)

**可用于“问答”**

![为“问答”使用多个表](media/desktop-qna-in-reports/desktop-qna_20.png)

### <a name="add-synonyms-to-tables-and-columns"></a>将同义词添加到表和列

此步骤专门应用于“问答”（一般情况下不应用于 Power BI 报表）。 用户通常会使用各种不同的术语来表示同一个意思，例如，销售总额、销售净额、净销售总额。 Power BI 的模型允许将这些同义词添加到模型内的表和列。 

此步骤非常重要。 即使使用直观的表和列名称，问答功能的用户仍会使用他们首先想到的词汇进行提问，而不是从列的预定义列表中选择词汇。 能够添加的有意义的同义词越多，用户使用报表的体验就越好。 要添加同义词，请在“关系”视图中，选择功能区中的“同义词”按钮，如下图中所示。

![为“问答”添加同义词](media/desktop-qna-in-reports/desktop-qna_21.png)

“同义词”字段显示在 Power BI Desktop 的右侧，可以在此处添加你的同义词，如下图中所示。

![为“问答”添加同义词](media/desktop-qna-in-reports/desktop-qna_22.png)

 添加同义词时需要留意，因为向多个列或表添加相同的同义词可能会引起歧义。 “问答”将尽可能利用上下文在不确定的同义词之间进行选择，但并不是所有问题都具有充分的上下文。 例如，当用户提问“计数客户”时，如果在你的模型中对“客户”有三个同义词，则用户可能无法获得他们想要寻找的问题的答案。 在这些情况下，请确保主要同义词的唯一性，因为将用于再次声明。 它可以提醒用户出现歧义（例如，再次声明“显示已存档的客户记录数”），提示用户以不同的方式来提问。


## <a name="next-steps"></a>后续步骤
有关 Power BI Desktop 中提供的功能的详细信息，请参阅以下文章：

* [在 Power BI Desktop 中使用钻取](desktop-drillthrough.md)
* [在“焦点”模式下显示仪表板磁贴或报表视觉对象](consumer/end-user-focus.md)

