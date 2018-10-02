---
title: 图形事件调用堆栈 |Microsoft Docs
ms.custom: ''
ms.date: 2018-06-30
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-debug
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- vs.graphics.callstack
ms.assetid: 8a30168d-8b39-4de1-b094-c7356ba101a3
caps.latest.revision: 12
author: mikejo5000
ms.author: mikejo
manager: ghogen
ms.openlocfilehash: d866e63ecffbed5cd5e9a9e477f99fbd8e8c3fc9
ms.sourcegitcommit: 55f7ce2d5d2e458e35c45787f1935b237ee5c9f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "47478424"
---
# <a name="graphics-event-call-stack"></a>图形事件调用堆栈
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

本主题的最新版本，请参阅[图形事件调用堆栈](https://docs.microsoft.com/visualstudio/debugger/graphics/graphics-event-call-stack)。  
  
Visual Studio 图形分析器中的图形事件调用堆栈可帮助你映射有问题的图形事件和应用的源代码之间的关系。  
  
 这就是“事件调用堆栈”窗口：  
  
 ![调用堆栈 DrawIndexed 事件。](../debugger/media/gfx-diag-demo-graphics-event-call-stack-orientation.png "gfx_diag_demo_graphics_event_call_stack_orientation")  
  
## <a name="understanding-the-graphics-event-call-stack"></a>了解图形事件调用堆栈  
 你可以使用“事件调用堆栈”来了解导致特定 Direct3D 事件的执行流。 它类似于 Visual Studio 调用堆栈窗口，不同之处在于，它不会显示正在运行的应用中活动线程的当前调用堆栈，而是在选定的 Direct3D 事件发生时显示存在的调用堆栈。 你可以从“事件调用堆栈”跳转到选定的 Direct3D 事件的调用站点，以检查周围的代码。  
  
 通过使用“事件调用堆栈”标识问题事件源自的代码路径，你可以使用你的代码库知识推导出问题可能的来源，或者可以在应用源代码中添加断点，以便使用传统调试技术检查应用或事件参数的状态是如何导致事件错误行为的。 这一检查可以帮助你在源代码中找到问题，这些问题只显示为呈现问题。  
  
### <a name="graphics-event-call-stack-information"></a>图形事件调用堆栈信息  
 调用堆栈不支持预框架事件或用户定义的事件。 图形事件的调用堆栈以表格形式显示。  
  
|列|描述|  
|------------|-----------------|  
|**名称**|包含调用站点的函数的唯一标识符号。 这个函数的调试符号在它可用时显示；否则将显示函数偏移量。|  
|**文件**|包含调用站点的源代码文件或库文件的文件名。|  
|**位置**|调用站点行号。|  
  
### <a name="links-to-graphics-objects"></a>指向图形对象的链接  
 若要了解选定的图形事件，你可能需要了解与它关联的 Direct3D 对象的有关信息。 **图形事件调用堆栈**窗口提供此信息的链接。  
  
## <a name="see-also"></a>请参阅  
 [演练：因顶点着色而缺少对象](../debugger/walkthrough-missing-objects-due-to-vertex-shading.md)


