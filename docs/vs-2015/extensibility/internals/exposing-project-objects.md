---
title: 公开项目对象 |Microsoft Docs
ms.custom: ''
ms.date: 2018-06-30
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-sdk
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- project objects, exposing
- extensibility, project objects
ms.assetid: 5bb24967-434a-4ef4-87a0-2f3250c9e22d
caps.latest.revision: 18
ms.author: gregvanl
manager: ghogen
ms.openlocfilehash: 5514589660df1850dc2f5d9fce3079f6769ec06e
ms.sourcegitcommit: 55f7ce2d5d2e458e35c45787f1935b237ee5c9f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "47468762"
---
# <a name="exposing-project-objects"></a>公开项目对象
[!INCLUDE[vs2017banner](../../includes/vs2017banner.md)]

本主题的最新版本，请参阅[公开项目对象](https://docs.microsoft.com/visualstudio/extensibility/internals/exposing-project-objects)。  
  
自定义项目类型可以提供自动化对象，以允许使用自动化接口项目的访问权限。 每个项目类型需要提供标准<xref:EnvDTE.Project>自动化对象，从访问<xref:EnvDTE.Solution>，其中包含在 IDE 中打开的所有项目的集合。 应在项目中每个项由公开<xref:EnvDTE.ProjectItem>对象使用访问<xref:EnvDTE.Project.ProjectItems>。 除了这些标准自动化对象，可以选择项目以提供特定于项目的自动化对象。  
  
 可以创建自定义根级别的自动化对象可以访问从根 DTE 对象使用后期绑定`DTE.<customeObjectName>`或`DTE.GetObject(“<customObjectName>”)`。 例如，Visual c + + 创建名为"VCProjects"可使用 DTE 访问 c + + 特定于项目的项目集合。VCProjects 或 DTE。GetObject("VCProjects")。 此外可以创建 Project.Object，这是唯一的项目类型，Project.CodeModel，可查询对 ProjectItem，公开 ProjectItem.Object 和 ProjectItem.FileCodeModel 其派生程度最高的对象。  
  
 它是公开的自定义的特定于项目的项目集合的项目的通用约定。 例如，[!INCLUDE[vcprvc](../../includes/vcprvc-md.md)]创建一个 c + + 特定项目集合，然后就可以访问使用`DTE.VCProjects`或`DTE.GetObject("VCProjects")`。 此外可以创建`Project.Object`，这是唯一的项目类型， `Project.CodeModel`，其派生程度最高的对象，这能查询`ProjectItem`，这会公开`ProjectItem.Object`，和一个`ProjectItem.FileCodeModel`。  
  
### <a name="to-contribute-a-vspackage-specific-object-for-a-project"></a>若要参与项目的特定于 VSPackage 的对象  
  
1.  将相应的密钥添加到你的 VSPackage 的.pkgdef 文件。  
  
     例如，下面是 c + + 语言项目的.pkgdef 设置：  
  
    ```  
    [$RootKey$\Packages\{F1C25864-3097-11D2-A5C5-00C04F7968B4}\Automation]  
    "VCProjects"=""  
    [$RootKey$\Packages\{F1C25864-3097-11D2-A5C5-00C04F7968B4}\AutomationEvents]  
    "VCProjectEngineEventsObject"=""  
    ```  
  
2.  实现中的代码<xref:Microsoft.VisualStudio.Shell.Interop.IVsPackage.GetAutomationObject%2A>方法，如以下示例所示。  
  
    ```cpp  
    STDMETHODIMP CVsPackage::GetAutomationObject(  
    /* [in]  */ LPCOLESTR       pszPropName,   
    /* [out] */ IDispatch **    ppIDispatch)  
    {  
    ExpectedPtrRet(pszPropName);  
    ExpectedPtrRet(ppIDispatch);  
    *ppIDispatch = NULL;  
  
        if (m_fZombie)  
            return E_UNEXPECTED;  
  
        if (_wcsicmp(pszPropName, g_wszAutomationProjects) == 0)  
        {  
            return GetAutomationProjects(ppIDispatch);  
        }  
        else if (_wcsicmp(pszPropName, g_wszAutomationProjectsEvents) == 0)  
        {  
            return CAutomationEvents::GetAutomationEvents(ppIDispatch);  
        }  
        else if (_wcsicmp(pszPropName, g_wszAutomationProjectItemsEvents) == 0)  
        {  
            return CAutomationEvents::GetAutomationEvents(ppIDispatch);  
        }  
        return E_INVALIDARG;  
    }   
    ```  
  
     在代码中，`g_wszAutomationProjects`是项目集合的名称。 `GetAutomationProjects`方法创建对象，它实现`Projects`接口，并返回`IDispatch`对调用对象，如下面的代码示例中所示的指针。  
  
    ```cpp  
    HRESULT CVsPackage::GetAutomationProjects(/* [out] */ IDispatch ** ppIDispatch)  
    {  
        ExpectedPtrRet(ppIDispatch);  
        *ppIDispatch = NULL;  
  
        if (!m_srpAutomationProjects)  
        {  
            HRESULT hr = CACProjects::CreateInstance(&m_srpAutomationProjects);  
            IfFailRet(hr);  
            ExpectedExprRet(m_srpAutomationProjects != NULL);  
        }  
        return m_srpAutomationProjects.CopyTo(ppIDispatch);  
    }  
    ```  
  
     应选择您的自动化对象的唯一名称。 不可预测，名称冲突，冲突会导致冲突的对象名称以任意如果，则引发多个项目类型使用相同的名称。 应包含你公司的名称或自动化对象的名称及其产品名称的一些独特之处。  
  
     自定义`Projects`集合对象是您项目的自动化模型的其余部分的便捷入口点。 项目对象也是可从访问<xref:EnvDTE.Solution>项目集合。 创建相应的代码和注册表条目，它们提供与使用者后`Projects`对象集合，您的实现必须提供剩余的项目模型的标准对象。 有关详细信息，请参阅[项目建模](../../extensibility/internals/project-modeling.md)。  
  
## <a name="see-also"></a>请参阅  
 <xref:Microsoft.VisualStudio.Shell.Interop.IVsPackage.GetAutomationObject%2A>
