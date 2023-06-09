# 在 Flutter 中创建下拉列表

> 原文：<https://blog.logrocket.com/creating-dropdown-list-flutter/>

下拉菜单允许用户从可用值列表中选择一个值，它们是任何现代应用程序中常见的小部件。

例如，如果您有一个带有国家选择的表单，有两个小部件可以用来显示这些国家的详细信息。一个是单选按钮，允许选择单个值。另一个选项是下拉菜单。

在这种情况下，下拉菜单将是最好的小部件，因为您可以添加一个很大的国家列表，当用户选择一个特定的国家时，它只显示该选定的国家。因此，它将为最终用户提供更好的用户体验。

在本文中，我们将涵盖这些主题，并让您更好地了解如何在 Flutter 中创建和定制下拉菜单。

## 创建下拉列表

在 Flutter 中创建下拉列表主要需要两种类型的小部件。

1.  `DropdownButton`
2.  `DropdownMenuItem`

`DropdownButton`小部件包含几个必需的属性，我们需要这些属性来使 dropdown 起作用。主要的必需属性是`item`属性。`item`属性接受一列`DropdownMenuItem`小部件，这些小部件需要显示可供选择的可能选项。

在本例中，让我们创建一个包含国家名称列表的下拉列表。我将创建一个单独的方法，该方法将返回包含国家名称的`DropdownMenuItem`小部件列表:

```
List<DropdownMenuItem<String>> get dropdownItems{
  List<DropdownMenuItem<String>> menuItems = [
    DropdownMenuItem(child: Text("USA"),value: "USA"),
    DropdownMenuItem(child: Text("Canada"),value: "Canada"),
    DropdownMenuItem(child: Text("Brazil"),value: "Brazil"),
    DropdownMenuItem(child: Text("England"),value: "England"),
  ];
  return menuItems;
}

```

接下来，创建一个带有`items`属性的`DropdownButton`小部件，并设置我们刚刚创建的向下拉列表提供值的方法。确保将其创建为一个单独的[有状态小部件](https://blog.logrocket.com/difference-between-stateless-stateful-widgets-flutter/)，因为我们需要在后面的阶段改变下拉列表的状态。

运行应用程序，你会看到一个下拉小部件，但不能与它进行任何互动。

现在让我们为下拉列表设置一个初始选择的值。`DropdownButton`控件的`value`属性可以用来设置当前选中的项目，我们可以将`"USA"`设置为第一个选中的项目:

```
class _DropdownItemState extends State<DropdownItem> {
  String selectedValue = "USA";
  @override
  Widget build(BuildContext context) {
    return DropdownButton(
      value: selectedValue,
      items: dropdownItems
      );
  }
}

```

![Creating a Flutter Dropdown](img/9b1de2497fdb9a5aeaf88d6f35985d08.png)

现在你可以看到`"USA"`显示为一个选择值。但是，您仍然不能与下拉菜单进行任何交互。这是因为我们还没有实现当值改变时下拉菜单应该如何表现。下一节将解释如何处理这些值的变化。

## 识别下拉列表值更改

`onChange`回调可用于识别值的变化。它将返回选定的值，您可以通过为下拉列表设置新值来更改下拉列表的状态，如下所示:

```
DropdownButton(
      value: selectedValue,
      onChanged: (String? newValue){
        setState(() {
          selectedValue = newValue!;
        });
      },
      items: dropdownItems
      )

```

现在，您可以看到下拉列表按预期工作，您可以从下拉列表中选择一个新值。

![Identifying dropdown value changes in Flutter](img/942cd8a3364b06457f544129cf6b922a.png)

## 禁用下拉菜单

将`onChange`设置为`null`将禁用下拉菜单。如果您设置了值属性，即使下拉列表被禁用，它也会将该值显示为选定值:

```
     DropdownButton(
        value: selectedValue,
        style: TextStyle(color: Colors.red,fontSize: 30),
        onChanged: null,
        items: dropdownItems
      )

```

![Disabled Dropdown Showing a Selection](img/6fab3af58d40b75b5c350a72737c65ca.png)

如果您想在下拉菜单被禁用时显示一个占位符文本，请使用`disabledHint`属性。使用该属性时，确保没有设置`value`属性:

```
DropdownButton(
      disabledHint: Text("Can't select"),
      style: TextStyle(color: Colors.red,fontSize: 30),
      onChanged: null,
      value:null.
      items: dropdownItems
      )

```

![Disabled Dropdown with Placeholder Text](img/b8f5e2ab7fc954eb9717d435051f3f10.png)

## 设置下拉列表的样式

### 应用图标

通过设置`DropdownButton`的`icon`属性，可以将图标应用于下拉菜单:

```
Widget build(BuildContext context) {
    return DropdownButton(
      value: selectedValue,
      icon: Icon(Icons.flag),
      onChanged: (String? newValue){
        setState(() {
          selectedValue = newValue!;
        });
      },
      items: dropdownItems
      );
  }

```

![Dropdown with an Icon](img/eef5048c43adf92fe8f6fc94cd606c15.png)

### 更改下拉项目的颜色

属性将允许你设置下拉菜单的背景颜色。这只会改变下拉项目的背景颜色，而不会改变选择按钮的颜色:

```
DropdownButton(
      value: selectedValue,
      dropdownColor: Colors.green,
      onChanged: (String? newValue){
        setState(() {
          selectedValue = newValue!;
        });
      },
      items: dropdownItems
      )

```

![Dropdown with Color Change](img/bc5ff966af5cdf5850ae14aad408192d.png)

### 更改文本颜色和大小

属性将允许你改变文本相关的样式，包括颜色和大小。您可以使用`TextStyle`小部件来设置下拉项目的文本相关样式:

```
DropdownButton(
      value: selectedValue,
      style: TextStyle(color: Colors.red,fontSize: 30),
      onChanged: (String? newValue){
        setState(() {
          selectedValue = newValue!;
        });
      },
      items: dropdownItems
      )

```

![Dropdown with Red Text Color](img/2ef9fb07efa31849083ed206b2ddb59f.png)

## `DropdownButtonFormField`和`DropdownButton`的区别

`DropdownButtonFormField`提供比普通`DropdownButton`小工具更多的功能。
首先，如果您需要自定义下拉菜单的外观，您可以通过设置`DropdownButtonFormField`小部件的`decoration`属性来设置自定义装饰:

```
              DropdownButtonFormField(
                decoration: InputDecoration(
                  enabledBorder: OutlineInputBorder(
                    borderSide: BorderSide(color: Colors.blue, width: 2),
                    borderRadius: BorderRadius.circular(20),
                  ),
                  border: OutlineInputBorder(
                    borderSide: BorderSide(color: Colors.blue, width: 2),
                    borderRadius: BorderRadius.circular(20),
                  ),
                  filled: true,
                  fillColor: Colors.blueAccent,
                ),
                dropdownColor: Colors.blueAccent,
                value: selectedValue,
                onChanged: (String? newValue) {
                  setState(() {
                    selectedValue = newValue!;
                  });
                },
                items: dropdownItems)

```

在这个例子中，如果你想设置一个背景颜色，你必须首先设置`InputDecoration`的`filled`属性，并将颜色设置为`fillColor`。否则，它不会显示正确的结果。

![Dropdown Menu with Custom Decoration](img/1b49bfe62f99dcd185061e4d8dede309.png)

`DropdownButtonFormField`中另一个有用的特性是内置的验证支持。

要实现这一点，您必须在一个`Form`小部件中使用这个小部件。在这个例子中，它将检查下拉列表是否有值，如果没有，它将在下拉列表下显示指定的消息。

当按钮调用类似`_dropdownFormKey.currentState!.validate()`的验证时，将触发该验证:

```
 class _DropdownItemState extends State<DropdownItem> {
  String? selectedValue = null;
  final _dropdownFormKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    return Form(
        key: _dropdownFormKey,
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            DropdownButtonFormField(
                decoration: InputDecoration(
                  enabledBorder: OutlineInputBorder(
                    borderSide: BorderSide(color: Colors.blue, width: 2),
                    borderRadius: BorderRadius.circular(20),
                  ),
                  border: OutlineInputBorder(
                    borderSide: BorderSide(color: Colors.blue, width: 2),
                    borderRadius: BorderRadius.circular(20),
                  ),
                  filled: true,
                  fillColor: Colors.blueAccent,
                ),
                validator: (value) => value == null ? "Select a country" : null,
                dropdownColor: Colors.blueAccent,
                value: selectedValue,
                onChanged: (String? newValue) {
                  setState(() {
                    selectedValue = newValue!;
                  });
                },
                items: dropdownItems),
            ElevatedButton(
                onPressed: () {
                  if (_dropdownFormKey.currentState!.validate()) {
                    //valid flow
                  }
                },
                child: Text("Submit"))
          ],
        ));
  }
}

```

![Validation Support in a Flutter Dropdown](img/52b14301171b8b96607eae4cf1fc67e4.png)

## 结论

在你的 [Flutter 应用](https://blog.logrocket.com/pros-and-cons-of-flutter-app-development/)中，可以使用`Dropdown`小部件来显示并从一大组选项中选择一个值。

如果你使用一个不需要验证的下拉菜单，你可以使用`DropdownButton`。

如果需要应用验证，并且下拉菜单在`Form`小部件下，最好使用`DropdownButtonFormField`,因为它有更多的定制和内置的验证支持。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)