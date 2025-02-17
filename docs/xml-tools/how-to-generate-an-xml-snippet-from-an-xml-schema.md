---
title: 如何：从 XML 架构生成 XML 代码段
ms.date: 11/04/2016
ms.topic: conceptual
ms.assetid: 2c128d2a-aaa6-4814-aa95-e07056afe338
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
- multiple
ms.openlocfilehash: e3795bbe8a200b868687cdb8da053bc078b7f14c
ms.sourcegitcommit: 75807551ea14c5a37aa07dd93a170b02fc67bc8c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67825754"
---
# <a name="how-to-generate-an-xml-snippet-from-an-xml-schema"></a>如何：从 XML 架构生成 XML 代码段

XML 编辑器具有从 XML 架构定义语言 (XSD) 架构生成 XML 代码段的功能。 例如，在编写 XML 文件，而定位元素名的旁边，可以按**选项卡**来填充该元素与从该元素的架构信息生成的 XML 数据。

此功能只适用于元素。 下列规则也适用：

- 元素必须具有关联的架构类型；即元素必须符合某个关联的架构。 架构类型不能是抽象的，并且该类型必须包含必选的属性和/或必选的子元素。

- 编辑器中的当前元素必须是空的，不包含任何属性。 例如，下列元素均有效

  - `<Account`

  - `<Account>`

  - `<Account></Account>`

- 光标必须紧邻元素名右侧。

生成的代码段包含所有必选的属性和元素。 如果 `minOccurs` 大于 1，代码段中将包括该元素所需的最小实例数，最多可达 100 个实例。 架构中发现的任何固定值都会使代码段中生成固定值。 `xsd:any` 和 `xsd:anyAttribute` 元素将被忽略，不生成任何其他代码段构造。

将生成默认值并标记为可编辑值。 如果架构指定了默认值，将使用此默认值。 但是，如果架构的默认值是空字符串，编辑器将通过以下方式生成默认值：

- 如果架构类型包含任何枚举方面（直接或间接地利用联合类型的任何成员），架构对象模型中发现的第一个枚举值将作为默认值使用。

- 如果架构类型为原子类型，编辑器将获取原子类型并插入原子类型名称。 对于派生的简单类型，将使用基简单类型。 对于列表类型，原子类型为 `itemType`。 对于联合，原子类型为第一个 `memberType` 的原子类型。

## <a name="example"></a>示例

 在本部分中的步骤说明如何使用 XML 编辑器的架构生成 XML 代码段功能。

> [!NOTE]
> 在开始下列步骤之前，先将架构文件保存到本地计算机上。

### <a name="to-create-a-new-xml-file-and-associate-it-with-an-xml-schema"></a>若要创建新的 XML 文件并将它与 XML 架构相关联

1. 上**文件**菜单，依次指向**新建**，然后单击**文件**。

2. 选择**XML 文件**中**模板**窗格，然后单击**打开**。

     新文件将在编辑器中打开。 该文件包含默认的 XML 声明 `<?xml version="1.0" encoding="utf-8">`。

3. 在文档属性窗口中，单击浏览按钮 ( **...** ) 上**架构**字段。

     **XSD 架构**显示对话框。

4. 单击 **添加**。

     **打开 XSD 架构**显示对话框。

5. 选择架构文件，然后单击**打开**。

6. 单击 **“确定”** 。

     现在是 XML 架构与 XML 文档关联。

### <a name="to-generate-an-xml-snippet"></a>生成 XML 代码段

1. 在编辑器窗格中键入 `<`。

2. 成员列表中显示可能的项：

     **！-** 添加注释。

     **!DOCTYPE**添加文档类型。

     **?** 若要添加的处理指令。

     **请联系**添加根元素。

3. 选择**联系人**成员列表然后按**Enter**。

     编辑器将添加开始标记 `<Contact` 并将光标置于元素名的后面。

4. 按**选项卡**生成的 XML 数据`Contact`元素基于其架构信息。

## <a name="input"></a>输入

 以下架构文件供该演练使用。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<xs:schema elementFormDefault="qualified"
           xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:simpleType name="phoneType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Voice"/>
      <xs:enumeration value="Fax"/>
      <xs:enumeration value="Pager"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="Contact">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="Name">
          <xs:simpleType>
            <xs:restriction base="xs:string"></xs:restriction>
          </xs:simpleType>
        </xs:element>
        <xs:element name="Title"
                    type="xs:string" />
        <xs:element name="Phone"
                    minOccurs="1"
                    maxOccurs="unbounded">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="Number"
                          minOccurs="1">
                <xs:simpleType>
                  <xs:restriction base="xs:string"></xs:restriction>
                </xs:simpleType>
              </xs:element>
              <xs:element name="Type"
                          default="Voice"
                          minOccurs="1"
                          type="phoneType"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

### <a name="output"></a>Output

 以下是根据与 `Contact` 元素关联的架构信息生成的 XML 数据。 项目标记为`bold`指定 XML 代码段中的可编辑字段。

```xml
<Contact>
  <Name>name</Name>
  <Title>title</Title>
  <Phone>
    <Number>number</Number>
    <Type>Voice</Type>
  </Phone>
</Contact>
```

## <a name="see-also"></a>请参阅

- [XML 代码段](../xml-tools/xml-snippets.md)
- [如何：使用 XML 代码段](../xml-tools/how-to-use-xml-snippets.md)
