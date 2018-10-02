---
title: 如何：向三维模型应用着色器 | Microsoft Docs
ms.custom: ''
ms.date: 2018-06-30
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-general
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: a3877bd6-abd8-4a9d-842c-6848b6c2f335
caps.latest.revision: 17
author: gewarren
ms.author: gewarren
manager: ghogen
ms.openlocfilehash: 277711a0dd2a106c8770b2b7d720666589e65d86
ms.sourcegitcommit: 55f7ce2d5d2e458e35c45787f1935b237ee5c9f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "47480652"
---
# <a name="how-to-apply-a-shader-to-a-3-d-model"></a>如何：向三维模型应用着色器
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

本主题的最新版本，请参阅[如何： 向三维模型应用着色器](https://docs.microsoft.com/visualstudio/designers/how-to-apply-a-shader-to-a-3-d-model)。  
  
本文档演示如何使用模型编辑器将定向关系图着色器语言 (DGSL) 着色器应用于三维模型。  
  
 本文档演示了此活动：  
  
-   向三维模型应用着色器  
  
## <a name="applying-a-shader-to-a-3-d-model"></a>向三维模型应用着色器  
 可对三维模型应用着色效果，使其具有有趣的外观。  
  
 开始前，请确保显示“属性”窗口”。  
  
#### <a name="to-apply-a-shader-to-a-3-d-model"></a>向三维模型应用着色器  
  
1.  从包含一个或多个模型的三维场景开始。 如果没有合适的三维场景，请按照[如何：创建基本三维模型](../designers/how-to-create-a-basic-3-d-model.md)中所述进行创建。 还必须具有可应用于模型的 DGSL 着色器。 如果没有合适的着色器，请按照[如何：创建基本颜色着色器](../designers/how-to-create-a-basic-color-shader.md)中所述进行创建，并在继续操作前确保已将其保存至文件中。  
  
2.  在“选择”模式中，选择要应用着色器的模型，然后在“属性”窗口的“效果”属性组中的 **Filename** 属性中，指定要应用到模型的 DGSL 着色器。  
  
 以下模型应用了基本颜色效果：  
  
 ![显示基本颜色效果的三维场景](../designers/media/digit-3d-model-effect.png "Digit-3D-Model-Effect")  
  
 将着色器应用到模型后，可在着色器设计器中打开它，方式是选择模型，然后在“属性”窗口的“效果”属性组的 **(Advanced)** 属性中选择省略号 (**...**) 按钮。  
  
## <a name="see-also"></a>请参阅  
 [如何：创建基本三维模型](../designers/how-to-create-a-basic-3-d-model.md)   
 [如何：创建基本颜色着色器](../designers/how-to-create-a-basic-color-shader.md)   
 [模型编辑器](../designers/model-editor.md)   
 [着色器设计器](../designers/shader-designer.md)


