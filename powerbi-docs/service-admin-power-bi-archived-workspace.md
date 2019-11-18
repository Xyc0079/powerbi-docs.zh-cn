---
title: Power BI 存档工作区
description: 管理 Office 365 租户之后的 Power BI 存档工作区
author: mgblythe
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-admin
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: mblythe
LocalizationGroup: Administration
ms.openlocfilehash: 66bf203cad577df0c985fbd73bcab3c6d79f6a95
ms.sourcegitcommit: 64c860fcbf2969bf089cec358331a1fc1e0d39a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2019
ms.locfileid: "73873569"
---
# <a name="power-bi-archived-workspace"></a>Power BI 存档工作区

> [!IMPORTANT]
> Power BI 不再支持存档的工作区功能，该功能将在 2019 年底删除。 如果你使用的是存档的工作区，应立即在当前租户中的新工作区中重新创建任何要保留的内容。 无法继续访问存档的工作区。 Power BI 不再支持相关功能，[Azure AD 租户的外部接管](service-admin-faq.md#what-is-the-process-to-manage-a-tenant-created-by-microsoft-for-my-users)。

借助 Power BI，任何人都可以在几分钟内进行注册并开始使用服务。  稍后，组织的 IT 部门可能会选择控制如何为组织中的用户管理 Power BI。  如果发生此控制，你便会受益于集中管理组织中的用户和权限。 还可以利用对组织中的其他服务使用的相同用户名和密码，受益于简化的登录流程。

在 IT 部门开始管理 Power BI 之前创建的任何内容都会置于 Power BI 存档工作区中，可从 [Power BI](https://app.powerbi.com) 的导航窗格访问该工作区。 应在“我的工作区”中开始创建新 Power BI 内容，这由组织的 IT 部门进行保护和管理。  存档工作区会继续存在，但是对于可以对存档工作区中的内容执行的操作存在一些限制。  必须将内容从存档工作区迁移到 IT 部门管理的“我的工作区”，才能删除这些限制。

## <a name="restrictions-in-your-archived-workspace"></a>存档工作区中的限制

Power BI 不会删除存档工作区中的内容。 可以继续获取数据、创建报表和仪表板以及刷新数据集。 作为内容共享对象的现有用户仍能查看自己存档工作区中的内容。 但是，对存档工作区中的内容存在一些限制：

* **OneDrive for Business**：对于存档工作区中的数据集，无法再从 OneDrive for Business 获取数据或刷新数据。  如果尝试连接到此源，便会看到警告。

* **共享仪表板**：无法在存档工作区中与其他用户共享仪表板。  任何已拥有访问权限的用户都能继续访问自己的存档工作区，从而查看共享仪表板。

* **创建组**：无法在存档工作区中创建组。

* **对 Power BI 移动应用的访问权限**：虽然仍可以在存档工作区中查看 Web 内容，但此内容将不再出现在 Power BI 移动应用中。

## <a name="migrating-content-in-your-archived-workspace"></a>迁移存档工作区中的内容

若要继续使用 Power BI，应在“我的工作区”中新建内容。 还应计划将存档工作区中的所有内容都迁移到“我的工作区”。  迁移内容的方式取决于内容类型：

* **Excel 或 Power BI Desktop 数据集**：从存档工作区切换到“我的工作区”，然后重新上传 Excel 或 Power BI Desktop 文件（通过选择“我的数据”  按钮），可以迁移这些数据集。  如果设置了定期刷新，必须为“我的工作区”中的新数据集重新配置这些设置。

* **其他数据集**：切换到“我的工作区”，然后选择“获取数据”  按钮，重新连接在存档工作区中创建的其他任何数据集。  可能需要重新输入安全或连接信息。

* **报表**：在你重新上传相应 Excel 或 Power BI Desktop 文件后，Excel 或 Power BI Desktop 文件中包含的报表便会自动重新创建。 当你重新连接到内容包时，作为内容包的一部分安装的报表也会重新创建。 如果通过 Power BI 服务创建了你自己的报表，请在“我的工作区”中重新创建这些报表。

* **仪表板**：当你重新连接到“我的工作区”中的内容包时，作为内容包的一部分安装的仪表板会自动重新创建。 如果通过 Power BI 服务创建了你自己的仪表板，请在“我的工作区”中重新创建这些仪表板。

更多问题？ [尝试咨询 Power BI 社区](https://community.powerbi.com/)

