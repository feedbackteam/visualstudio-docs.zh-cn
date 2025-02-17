---
title: 编码的 UI 测试剖析 | Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-test
ms.topic: conceptual
helpviewer_keywords:
- coded UI tests
ms.assetid: 9c5d82fc-3fb7-4bb1-a9ac-ac1fa3a4b500
caps.latest.revision: 25
ms.author: gewarren
manager: jillfra
ms.openlocfilehash: 46226d241db04cad0f4859650dc2aff855b72f08
ms.sourcegitcommit: 94b3a052fb1229c7e7f8804b09c1d403385c7630
ms.translationtype: MTE95
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "68155609"
---
# <a name="anatomy-of-a-coded-ui-test"></a>编码的 UI 测试剖析
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

在代码 UI 测试项目中创建编码的 UI 测试时，将向解决方案添加多个文件。 在本主题中，我们将使用示例“编码的 UI 测试”来浏览这些文件。  
  
 **要求**  
  
- Visual Studio Enterprise  
  
## <a name="contents-of-a-coded-ui-test"></a>编码的 UI 测试的内容  
 创建编码的 UI 测试时，**编码的 UI 测试生成器**会创建受测用户界面的映射，以及所有测试的测试方法、参数和断言。 此外，它还会为每个测试创建一个类文件。  
  
|文件|内容|可编辑？|  
|----------|--------------|---------------|  
|[UIMap.Designer.cs](#UIMapDesignerFile)|[声明部分](#UIMapDesignerFile)<br /><br /> [UIMap 类](#UIMapClass)（自动生成的分部类）<br /><br /> [方法](#UIMapMethods)<br /><br /> [属性](#UIMapProperties)|No|  
|[UIMap.cs](#UIMapCS)|[UIMap 类](#UIMapCS)（分部类）|是|  
|[CodedUITest1.cs](#CodedUITestCS)|[CodedUITest1 类](#CodedUITestCS)<br /><br /> [方法](#CodedUITestMethods)<br /><br /> [属性](#CodedUITestProperties)|是|  
|[UIMap.uitest](#UIMapuitest)|用于测试的 UI 的 XML 映射。|No|  
  
### <a name="UIMapDesignerFile"></a>UIMap.Designer.cs  
 此文件包含在测试创建后由**编码的 UI 测试生成器**自动创建的代码。 此文件会在每次更改测试时重新创建，因此，你不可以在其中添加或修改代码。  
  
#### <a name="declarations-section"></a>声明部分  
 本部分包含 Windows UI 的以下声明。  
  
```csharp  
using System;  
using System.CodeDom.Compiler;  
using System.Collections.Generic;  
using System.Drawing;  
using System.Text.RegularExpressions;  
using System.Windows.Input;  
using Microsoft.VisualStudio.TestTools.UITest.Extension;  
using Microsoft.VisualStudio.TestTools.UITesting;  
using Microsoft.VisualStudio.TestTools.UITesting.WinControls;  
using Microsoft.VisualStudio.TestTools.UnitTesting;  
using Keyboard = Microsoft.VisualStudio.TestTools.UITesting.Keyboard;  
using Mouse = Microsoft.VisualStudio.TestTools.UITesting.Mouse;  
using MouseButtons = System.Windows.Forms.MouseButtons;  
```  
  
 包含用于 Windows 用户界面 (UI) 的 <xref:Microsoft.VisualStudio.TestTools.UITesting.WinControls> 命名空间。 对于网页 UI，命名空间将为 <xref:Microsoft.VisualStudio.TestTools.UITesting.HtmlControls>；对于 Windows Presentation Foundation UI，命名空间将为 <xref:Microsoft.VisualStudio.TestTools.UITesting.WpfControls>。  
  
#### <a name="UIMapClass"></a>UIMap 类  
 该文件的下一部分是 <xref:Microsoft.VisualStudio.TestTools.UITest.Common.UIMap.UIMap> 类。  
  
```  
[GeneratedCode("Coded UITest Builder", "10.0.21221.0")]  
public partial class UIMap  
```  
  
 类代码开头为 <xref:System.CodeDom.Compiler.GeneratedCodeAttribute>，可应用于作为分部类声明的类。 你将注意到，该属性也适用于此文件中的每个类。 另一个可以包含此类的更多代码的文件是 `UIMap.cs`，稍后将作介绍。  
  
 生成的 `UIMap` 类包括记录测试时指定的每一种方法的代码。  
  
```  
public void LaunchCalculator()  
public void AddItems()  
public void VerifyTotal()  
public void CleanUp()  
```  
  
 <xref:Microsoft.VisualStudio.TestTools.UITest.Common.UIMap.UIMap> 类的此部分还包括为方法所需的每种属性生成的代码。  
  
```  
public virtual LaunchCalculatorParams LaunchCalculatorParams  
public virtual AddItemsParams AddItemsParams  
public virtual VerifyTotalExpectedValues VerifyTotalExpectedValues  
public virtual CalculateItemsParams CalculateItemsParams  
public virtual VerifyMathAppTotalExpectedValues   
    VerifyMathAppTotalExpectedValues  
public UIStartMenuWindow UIStartMenuWindow  
public UIRunWindow UIRunWindow  
public UICalculatorWindow UICalculatorWindow  
public UIStartWindow UIStartWindow  
public UIMathApplicationWindow UIMathApplicationWindow  
```  
  
##### <a name="UIMapMethods"></a>UIMap 方法  
 每种方法都具有类似于 `AddItems()` 方法的结构。 这将在代码中详细介绍，为清楚起见，代码中添加了换行符。  
  
```  
/// <summary>  
/// AddItems - Use 'AddItemsParams' to pass parameters into this method.  
/// </summary>  
public void AddItems()  
{  
    #region Variable Declarations  
    WinControl uICalculatorDialog =   
        this.UICalculatorWindow.UICalculatorDialog;  
    WinEdit uIItemEdit =   
        this.UICalculatorWindow.UIItemWindow.UIItemEdit;  
    #endregion  
  
    // Type '{NumPad7}' in 'Calculator' Dialog  
    Keyboard.SendKeys(uICalculatorDialog,   
        this.AddItemsParams.UICalculatorDialogSendKeys,   
        ModifierKeys.None);  
  
    // Type '{Add}{NumPad2}{Enter}' in 'Unknown Name' text box  
    Keyboard.SendKeys(uIItemEdit,   
        this.AddItemsParams.UIItemEditSendKeys,   
        ModifierKeys.None);  
}  
```  
  
 每种方法定义的总结评论介绍了哪些类可用于该方法的参数值。 在此示例中，为 `AddItemsParams` 类，该类在后面的 `UIMap.cs` 文件中定义，并且也是由 `AddItemsParams` 属性返回的值类型。  
  
 方法代码的顶部是 `Variable Declarations` 区域，该区域用于定义将被该方法使用的 UI 对象的局部变量。  
  
 在此方法中，`UIItemWindow` 和 `UIItemEdit` 属性都可以通过使用 `UICalculatorWindow` 类进行访问，该类在后面的 `UIMap.cs` 文件中定义。  
  
 接下来是可通过使用 `AddItemsParams` 对象的属性将文本从键盘发送到计算器应用程序的行。  
  
 `VerifyTotal()` 方法具有非常相似的结构，并包括以下声明代码。  
  
```  
// Verify that 'Unknown Name' text box's property 'Text' equals '9. '  
Assert.AreEqual(  
    this.VerifyTotalExpectedValues.UIItemEditText,   
    uIItemEdit.Text);  
```  
  
 由于 Windows 计算器应用程序的开发人员的未提供该控件公开可用的名称，因此文本框的名称被列为未知。 当实际值不等于预期值时，<xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.AreEqual%2A?displayProperty=fullName> 方法失败，从而导致测试失败。 另请注意，预期值的小数点后面有一个空格。 如果你永远都不会修改此特定测试的功能，则必须允许该小数点和空格。  
  
##### <a name="UIMapProperties"></a>UIMap 属性  
 每个属性的代码在整个类中都是非常标准的。 下面的 `AddItemsParams` 属性的代码用于 `AddItems()` 方法。  
  
```  
public virtual AddItemsParams AddItemsParams  
{  
    get  
    {  
        if ((this.mAddItemsParams == null))  
        {  
            this.mAddItemsParams = new AddItemsParams();  
        }  
        return this.mAddItemsParams;  
    }  
}  
```  
  
 请注意，该属性使用名为 `mAddItemsParams` 的专用本地变量在返回之前保留该值。 属性名称和它所返回的对象的类名称是相同的。 此类稍后将在 `UIMap.cs` 文件中介绍。  
  
 每个由属性返回的类结构上都相似。 以下为 `AddItemsParams` 类。  
  
```  
/// <summary>  
/// Parameters to be passed into 'AddItems'  
/// </summary>  
[GeneratedCode("Coded UITest Builder", "10.0.21221.0")]  
public class AddItemsParams  
{  
    #region Fields  
    /// <summary>  
    /// Type '{NumPad7}' in 'Calculator' Dialog  
    /// </summary>  
    public string UICalculatorDialogSendKeys = "{NumPad7}";  
  
    /// <summary>  
    /// Type '{Add}{NumPad2}{Enter}' in 'Unknown Name' text box  
    /// </summary>  
    public string UIItemEditSendKeys = "{Add}{NumPad2}{Enter}";  
    #endregion  
}  
```  
  
 正如 `UIMap.cs` 文件中的所有类一样，此类也是以 <xref:System.CodeDom.Compiler.GeneratedCodeAttribute> 开头。 在此示例中，小类是 `Fields` 区域，它定义了用作 <xref:Microsoft.VisualStudio.TestTools.UITesting.Keyboard.SendKeys%2A?displayProperty=fullName> 方法的参数的字符串，该方法是我们之前介绍的 `UIMap.AddItems()` 方法中使用的方法。 你可以在调用在其中使用这些参数的方法之前，编写代码以替换这些字符串字段中的值。  
  
### <a name="UIMapCS"></a>UIMap.cs  
 默认情况下，此文件包括没有方法或属性的分部 `UIMap` 类。  
  
#### <a name="uimap-class"></a>UIMap 类  
 这是你可以在其中创建自定义代码以扩展 <xref:Microsoft.VisualStudio.TestTools.UITest.Common.UIMap.UIMap> 类的功能的地方。 **编码的 UI 测试生成器**不会在每次测试有修改时重新生成你在此文件中创建的代码。  
  
 <xref:Microsoft.VisualStudio.TestTools.UITest.Common.UIMap.UIMap> 的所有部分都可以使用 <xref:Microsoft.VisualStudio.TestTools.UITest.Common.UIMap.UIMap> 类的任何其他部分的方法和属性。  
  
### <a name="CodedUITestCS"></a>CodedUITest1.cs  
 此文件由**编码的 UI 测试生成器**生成，但不会在每次测试有修改时重新创建，因此可以在此文件中修改代码。 文件的名称由你创建测试时为该测试指定的名称生成。  
  
#### <a name="codeduitest1-class"></a>CodedUITest1 类  
 默认情况下，此文件只包含一个类的定义。  
  
```  
[CodedUITest]  
public class CodedUITest1  
```  
  
 T:Microsoft.VisualStudio.TestTools.UITesting.CodedUITestAttribute 自动应用于类，该类允许测试框架将其识别为测试扩展。 另请注意，这不是一个分部类。 此文件中包含所有类代码。  
  
##### <a name="CodedUITestProperties"></a>CodedUITest1 属性  
 此类包含两个位于文件底部的默认属性。 它们不能修改。  
  
```  
/// <summary>  
/// Gets or sets the test context which provides  
/// information about and functionality for the current test run.  
///</summary>  
public TestContext TestContext  
public UIMap UIMap  
```  
  
##### <a name="CodedUITestMethods"></a>CodedUITest1 方法  
 默认情况下，此类只包含一种方法。  
  
```  
public void CodedUITestMethod1()  
```  
  
 此方法调用你在录制测试时指定的所有 `UIMap` 方法，[UIMap 类](#UIMapClass)部分中对此进行了介绍。  
  
 标题为 `Additional test attributes` 的区域（如果取消注释）包含两种可选方法。  
  
```  
// Use TestInitialize to run code before running each test   
[TestInitialize()]  
public void MyTestInitialize()  
{  
    // To generate code for this test, select "Generate Code for Coded   
    // UI Test" from the shortcut menu and select one of the menu items.  
    // For more information on generated code, see   
    // http://go.microsoft.com/fwlink/?LinkId=179463  
  
    // You could move this line from the CodedUITestMethod1() method  
    this.UIMap.LaunchCalculator();  
}  
  
// Use TestCleanup to run code after each test has run  
[TestCleanup()]  
public void MyTestCleanup()  
{  
    // To generate code for this test, select "Generate Code for Coded   
    // UI Test" from the shortcut menu and select one of the menu items.  
    // For more information on generated code, see   
    // http://go.microsoft.com/fwlink/?LinkId=179463  
  
    // You could move this line from the CodedUITestMethod1() method  
    this.UIMap.CloseCalculator();  
}  
```  
  
 `MyTestInitialize()` 方法具有应用于它的 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestInitializeAttribute>，它指示测试框架在调用任何其他测试方法之前调用此方法。 与此类似，`MyTestCleanup()` 方法具有应用于它的 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestCleanupAttribute>，它指示测试框架在调用所有其他测试方法之后调用此方法。 可以选择使用这些方法。 对于此测试，可以从 `MyTestInitialize()` 调用 `UIMap.LaunchCalculator()` 方法，从 `MyTestCleanup()` 而不是从 `CodedUITest1Method1()` 调用 `UIMap.CloseCalculator()` 方法。  
  
 如果使用 <xref:Microsoft.VisualStudio.TestTools.UITesting.CodedUITestAttribute> 将更多方法添加到此类，则测试框架将调用作为测试一部分调用每种方法。  
  
### <a name="UIMapuitest"></a>UIMap.uitest  
 这是一个 XML 文件，表示编码的 UI 测试录制及其所有部分的结构。 除类的方法和属性之外，其中还包括操作以及这些类。 [UIMap.Designer.cs](#UIMapDesignerFile) 文件包含编码的 UI 测试生成器为了重现测试结构而生成的代码，并建立与测试框架的连接。  
  
 `UIMap.uitest` 文件不是可直接编辑的文件。 不过，可以使用编码的 UI 测试生成器来修改测试，从而自动修改 `UIMap.uitest` 文件和 [UIMap.Designer.cs](#UIMapDesignerFile) 文件。  
  
## <a name="see-also"></a>另请参阅  
 <xref:Microsoft.VisualStudio.TestTools.UITest.Common.UIMap.UIMap>   
 <xref:Microsoft.VisualStudio.TestTools.UITesting.WinControls>   
 <xref:Microsoft.VisualStudio.TestTools.UITesting.HtmlControls>   
 <xref:Microsoft.VisualStudio.TestTools.UITesting.WpfControls>   
 <xref:System.CodeDom.Compiler.GeneratedCodeAttribute>   
 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.AreEqual%2A?displayProperty=fullName>   
 <xref:Microsoft.VisualStudio.TestTools.UITesting.Keyboard.SendKeys%2A?displayProperty=fullName>   
 <xref:Microsoft.VisualStudio.TestTools.UITesting.CodedUITestAttribute>   
 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestInitializeAttribute>   
 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestCleanupAttribute>   
 [使用 UI 自动化来测试代码](../test/use-ui-automation-to-test-your-code.md)   
 [创建编码的 UI 测试](../test/use-ui-automation-to-test-your-code.md#VerifyingCodeUsingCUITCreate)   
 [编码的 UI 测试的最佳做法](../test/best-practices-for-coded-ui-tests.md)   
 [使用多个 UI 映射测试大型应用程序](../test/testing-a-large-application-with-multiple-ui-maps.md)   
 [支持编码的 UI 测试和操作录制的配置和平台](../test/supported-configurations-and-platforms-for-coded-ui-tests-and-action-recordings.md)
