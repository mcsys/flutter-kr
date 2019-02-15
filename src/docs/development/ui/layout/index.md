---
title: Layouts in Flutter
short-title: Layout
description: Learn how Flutter's layout mechanism works and how to build a layout.
diff2html: true
---

{% assign api = 'https://docs.flutter.io/flutter' -%}
{% capture code -%} {{site.repo.this}}/tree/{{site.branch}}/src/_includes/code {%- endcapture -%}
{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}
{% assign rawExFile = 'https://raw.githubusercontent.com/flutter/website/master/examples' -%}
{% capture demo -%} {{site.repo.flutter}}/tree/{{site.branch}}/examples/flutter_gallery/lib/demo {%- endcapture -%}

<style>dl, dd { margin-bottom: 0; }</style>

{{site.alert.secondary}}
  <h4 class="no_toc">요점은 무엇인가요?</h4>

  * Widget 은 UI를 만들기 위해 사용하는 클래스입니다.
  * Widget 은 layout과 UI 요소 모두에 사용합니다.
  * 간단한 widget 을 조합하여 복잡한 widget 을 만듭니다.
{{site.alert.end}}

The core of Flutter's layout mechanism is widgets. In Flutter, almost
everything is a widget&mdash;even layout models are widgets.
The images, icons, and text that you see in a Flutter app  are all widgets.
But things you don't see are also widgets, such as the rows, columns,
and grids that arrange, constrain, and align the visible widgets.
플러터 레이아웃 메카니즘 핵심은 위젯입니다. 플러터에서, 거의 모든 것은 widget 입니다.
레이아웃 모델들 조차 widget 입니다.
플러터 앱 안에서 여러분이 볼 수 있는 이미지, 아이콘, 텍스트는 모두 widget 입니다.
그러나 여러분이 볼 수 없는 것들 또한 widget 입니다, 예를 들면 열, 행, 그리고 배치된 격자, 제약조건,
그리고 widget 의 정렬.

You create a layout by composing widgets to build more complex widgets.
For example, the first screenshot below shows 3 icons with a label under each one:
여러분은 widget을 조합하여 좀 더 복잡한 widget을 만들어 레이아웃을 만들수 있습니다.
예를 들면, 첫 번째 스크린샷 아래에는 각각 아래에 라벨이 있는 3개의 아이콘을 보여줍니다.

<div class="row mb-4">
  <div class="col-12 text-center">
    {% asset ui/layout/lakes-icons.png class="border mt-1 mb-1 mw-100" alt="Sample layout" %}
    {% asset ui/layout/lakes-icons-visual.png class="border mt-1 mb-1 mw-100" alt="Sample layout with visual debugging" %}
  </div>
</div>

The second screenshot displays the visual layout, showing a row of
3 columns where each column contains an icon and a label.
두 번째 스크린샷은 시각적인 레이아웃을 표시합니다,
각각 하나의 아이콘과 라벨을 포함하는 3개의 열이 하나의 행에 보여집니다.

{{site.alert.note}}
  Most of the screenshots in this tutorial are displayed with
  `debugPaintSizeEnabled` set to true so you can see the visual layout.
  For more information, see
  [Visual debugging](/docs/testing/debugging#visual-debugging), a section in
  [Debugging Flutter apps](/docs/testing/debugging).
{{site.alert.end}}

Here's a diagram of the widget tree for this UI:
다음은 이 UI를 위한 widget 트리의 다이어그램입니다.

{% asset ui/layout/sample-flutter-layout.png class="mw-100" alt="Node tree" %}
{:.text-center}

Most of this should look as you might expect, but you might be wondering
about the containers (shown in pink). [Container][] is a widget class that allows
you to customize its child widget. Use a `Container` when you want to
add padding, margins, borders, or background color, to name some of its
capabilities.
대부분은 여러분이 예상대로 보일 것입니다, 그러나 여러분은 아마 container 에 대해 궁금해 하실것입니다.
[Container][] 는 여러분이 Container 자식 widget을 사용자 정의할 수 있는 widget 클래스입니다.

In this example, each [Text][] widget is placed in a `Container` to add margins.
The entire [Row][] is also placed in a `Container` to add padding around the
row.
여기 예제에서, 각각의 [Text][] widget 은 마진을 추가하여 `Container` 안에 놓여집니다.
전체 [Row][] 또한, 행 주변에 패딩을 추가하여 `Container` 안에 놓여집니다.

The rest of the UI in this example is controlled by properties.
Set an [Icon][]'s color using its `color` property.
Use the `Text.style` property to set the font, its color, weight, and so on.
Columns and rows have properties that allow you to specify how their
children are aligned vertically or horizontally, and how much space
the children should occupy.
여기 예제에서 나머지 UI는 속성에 따라 제어됩니다. [Icon][]의 색상은 `color` 속성을 사용하여 설정합니다.
`Text.style` 속상을 사용해서 폰트, 색상, 비중 등등을 설정합니다.
행과 열은 어떻게 자식을 수직적 또는 수평적으로 정렬할 것인지, 얼마나 많은 여백을 사용할지 여러분이 정의할 수 있는 속성을 가집니다.


## Lay out a widget
## widget 배치

How do you layout a single widget in Flutter? This section shows you how to
create and display a simple widget. It also shows the entire code for a simple
Hello World app.
플러터에서 여러분은 어떻게 단일 widget을 배치할 것인가요? 이번 부분은 여러분이 어떻게 단일 widget을 만들고 표현할지 보여드립니다.
또한 간단한 Hello World app을 통해 전체 코드를 보여줍니다.

In Flutter, it takes only a few steps to put text, an icon, or an image on the
screen.
플러터에서, 단지 몇 단계만으로 화면에 문자, 아이콘, 이미지를 넣을 수 있습니다.

### 1. Select a layout widget
### 1. 레이아웃 widget 을 선택합니다.

Choose from a variety of [layout widgets][] based
on how you want to align or constrain the visible widget,
as these characteristics are typically passed on to the
contained widget.
정렬 또는 제약조건의 특성을 일반적으로 포함하고 있는 widget을 여러분이 원하는 정렬 또는 제약조건에 근거하여
여러 가지의 [layout widgets][] 선택합니다,

This example uses [Center][] which centers its content
horizontally and vertically.
이 예제는 [Center][] 사용합니다. 이것의 콘텐츠가 수평적 그리고 수직적으로 중심에 위치하도록 합니다

### 2. Create a visible widget
### 2. 가시적인 widget을 만듭니다.

For example, create a [Text][] widget:
예를 들면, [Text][] widget을 만듭니다:

<?code-excerpt "layout/base/lib/main.dart (text)" replace="/child: //g"?>
```dart
Text('Hello World'),
```

Create an [Image][] widget:
[Image][] widget을 만듭니다:

<?code-excerpt "layout/lakes/step5/lib/main.dart (Image-asset)" remove="/width|height/"?>
```dart
Image.asset(
  'images/lake.jpg',
  fit: BoxFit.cover,
),
```

Create an [Icon][] widget:
[Icon][] widget을 만듭니다:

<?code-excerpt "layout/lakes/step5/lib/main.dart (Icon)"?>
```dart
Icon(
  Icons.star,
  color: Colors.red[500],
),
```

### 3. Add the visible widget to the layout widget
### 3. 레이아웃 위젯에 가시적인 위젯을 추가합니다.

<?code-excerpt path-base="layout/base"?>

All layout widgets have either of the following:
모든 레이아웃 위젯은 다음중 하나를 가집니다:

- A `child` property if they take a single child -- for example, `Center` or
  `Container`
- A `children` property if they take a list of widgets -- for example, `Row`,
  `Column`, `ListView`, or `Stack`.
- `child` 속성이 하나의 자식을 가진다면 -- 예, `Center` 또는
  `Container`
- A `children` 속성의 여러 widget 목록을 가진다면 -- 예, `Row`,
  `Column`, `ListView`, 또는 `Stack`.

Add the `Text` widget to the `Center` widget:
`Text` widget 을 `Center` widget 에 추가합니다.:

<?code-excerpt "lib/main.dart (centered-text)" replace="/body: //g"?>
```dart
Center(
  child: Text('Hello World'),
),
```

### 4. Add the layout widget to the page
### 4. 레이아웃 widget을 page에 추가합니다

A Flutter app is itself a widget, and most widgets have a [build()][]
method. Instantiating and returning a widget in the app's `build()` method
displays the widget.
플러터 앱은 스스로가 widget입니다, 그리고 모든 widget은 [build()][] 메소드를 가집니다.
앱 `build()` method에서 초기화하고 반환된 widget은 widget을 표시합니다.


#### Material apps

For a `Material` app, you can use a [Scaffold][] widget; it provides a default
banner, background color, and has API for adding drawers, snack bars, and bottom
sheets. Then you can add the `Center` widget directly to the `body` property for
the home page.

<?code-excerpt path-base="layout/base"?>
<?code-excerpt "lib/main.dart (MyApp)" title?>
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter layout demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Flutter layout demo'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

{{site.alert.note}}
  The [Material library][] implements widgets that follow [Material
  Design][] principles. When designing your UI, you can exclusively use
  widgets from the standard [widgets library][], or you can use widgets from
  the Material library. You can mix widgets from both libraries, you can
  customize existing widgets, or you can build your own set of custom
  widgets.
{{site.alert.end}}

#### Non-Material apps

For a non-Material app, you can add the `Center` widget to the app's
`build()` method:

<?code-excerpt path-base="layout/non_material"?>
<?code-excerpt "lib/main.dart (MyApp)" title?>
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(color: Colors.white),
      child: Center(
        child: Text(
          'Hello World',
          textDirection: TextDirection.ltr,
          style: TextStyle(
            fontSize: 32,
            color: Colors.black87,
          ),
        ),
      ),
    );
  }
}
```

By default a non-Material app doesn't include an `AppBar`, title, or background
color. If you want these features in a non-Material app, you have to build them
yourself. This app changes the background color to white and the text to dark
grey to mimic a Material app.

<div class="row">
<div class="col-md-6" markdown="1">
  That's it! When you run the app, you should see _Hello World_.

  App source code:
  - [Material app]({{examples}}/layout/base)
  - [Non-Material app]({{examples}}/layout/non_material)
</div>
<div class="col-md-6">
  {% include app-figure.md img-class="site-mobile-screenshot border w-75"
      image="ui/layout/hello-world.png" alt="Hello World" %}
</div>
</div>

<hr>

## Lay out multiple widgets vertically and horizontally

<?code-excerpt path-base=""?>

One of the most common layout patterns is to arrange widgets vertically
or horizontally. You can use a Row widget to arrange widgets horizontally,
and a Column widget to arrange widgets vertically.

{{site.alert.secondary}}
  <h4 class="no_toc">What's the point?</h4>

  * Row and Column are two of the most commonly used layout patterns.
  * Row and Column each take a list of child widgets.
  * A child widget can itself be a Row, Column, or other complex widget.
  * You can specify how a Row or Column aligns its children, both vertically
    and horizontally.
  * You can stretch or constrain specific child widgets.
  * You can specify how child widgets use the Row's or Column's available space.
{{site.alert.end}}

To create a row or column in Flutter, you add a list of children widgets to a
[Row][] or [Column][] widget. In turn, each child can itself be a row or column,
and so on. The following example shows how it is possible to nest rows or
columns inside of rows or columns.

This layout is organized as a Row. The row contains two children:
a column on the left, and an image on the right:

{% asset ui/layout/pavlova-diagram.png class="mw-100"
    alt="Screenshot with callouts showing the row containing two children" %}

The left column's widget tree nests rows and columns.

{% asset ui/layout/pavlova-left-column-diagram.png class="mw-100"
    alt="Diagram showing a left column broken down to its sub-rows and sub-columns" %}

You'll implement some of Pavlova's layout code in
[Nesting rows and columns](#nesting-rows-and-columns).

{{site.alert.note}}
  Row and Column are basic primitive widgets for horizontal
  and vertical layouts&mdash;these low-level widgets allow for maximum
  customization. Flutter also offers specialized, higher level widgets
  that might be sufficient for your needs. For example, instead of Row
  you might prefer
  [ListTile]({{api}}/material/ListTile-class.html),
  an easy-to-use widget with properties for leading and trailing icons,
  and up to 3 lines of text.  Instead of Column, you might prefer
  [ListView]({{api}}/widgets/ListView-class.html),
  a column-like layout that automatically scrolls if its content is too long
  to fit the available space.  For more information,
  see [Common layout widgets](#common-layout-widgets).
{{site.alert.end}}

### Aligning widgets

You control how a row or column aligns its children using the
`mainAxisAlignment` and `crossAxisAlignment` properties.
For a row, the main axis runs horizontally and the cross axis runs
vertically. For a column, the main axis runs vertically and the cross
axis runs horizontally.

<div class="mb-2 text-center">
  {% asset ui/layout/row-diagram.png class="mb-2 mw-100"
      alt="Diagram showing the main axis and cross axis for a row" %}
  {% asset ui/layout/column-diagram.png class="mb-2 mr-2 ml-2 mw-100"
      alt="Diagram showing the main axis and cross axis for a column" %}
</div>

The [MainAxisAlignment]({{api}}/rendering/MainAxisAlignment-class.html)
and [CrossAxisAlignment]({{api}}/rendering/CrossAxisAlignment-class.html)
classes offer a variety of constants for controlling alignment.

{{site.alert.note}}
  When you add images to your project,
  you need to update the pubspec file to access them&mdash;this
  example uses `Image.asset` to display the images.  For more information,
  see this example's [pubspec.yaml
  file]({{examples}}/layout/row/pubspec.yaml),
  or [Adding Assets and Images in Flutter](/docs/development/ui/assets-and-images).
  You don't need to do this if you're referencing online images using
  `Image.network`.
{{site.alert.end}}

In the following example, each of the 3 images is 100 pixels wide.
The render box (in this case, the entire screen) is more than 300 pixels wide,
so setting the main axis alignment to `spaceEvenly` divides the free
horizontal space evenly between, before, and after each image.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/row_column/lib/main.dart (Row)" replace="/Row/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!Row!](
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-spaceevenly-visual.png class="mw-100" alt="Row with 3 evenly spaced images" %}

  **App source:** [row_column]({{examples}}/layout/row_column)
</div>
</div>

Columns work the same way as rows. The following example shows a column
of 3 images, each is 100 pixels high. The height of the render box
(in this case, the entire screen) is more than 300 pixels, so
setting the main axis alignment to `spaceEvenly` divides the free vertical
space evenly between, above, and below each image.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/row_column/lib/main.dart (Column)" replace="/Column/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!Column!](
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  {% endprettify %}

  **App source:** [row_column]({{examples}}/layout/row_column)
</div>
<div class="col-lg-4 text-center">
  {% asset ui/layout/column-visual.png class="mb-4" height="250px"
      alt="Column showing 3 images spaced evenly" %}
</div>
</div>

### Sizing widgets

When a layout is too large to fit a device, a yellow and black striped pattern
appears along the affected edge. Here is an [example][sizing] of a row that is
too wide:

{% asset ui/layout/layout-too-large.png class="mw-100" alt="Overly-wide row" %}
{:.text-center}

Widgets can be sized to fit within a row or column by using the [Expanded][]
widget. To fix the previous example where the row of images is too wide for its
render box, wrap each image with an `Expanded` widget.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/sizing/lib/main.dart (expanded-images)" replace="/Expanded/[!$&!]/g"?>
  {% prettify dart context="html" %}
  Row(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      [!Expanded!](
        child: Image.asset('images/pic1.jpg'),
      ),
      [!Expanded!](
        child: Image.asset('images/pic2.jpg'),
      ),
      [!Expanded!](
        child: Image.asset('images/pic3.jpg'),
      ),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-expanded-2-visual.png class="mw-100"
      alt="Row of 3 images that are too wide, but each is constrained to take only 1/3 of the space" %}

  **App source:** [sizing]({{examples}}/layout/sizing)
</div>
</div>

Perhaps you want a widget to occupy twice as much space as its siblings. For
this, use the `Expanded` widget `flex` property, an integer that determines the
flex factor for a widget. The default flex factor is 1. The following code sets
the flex factor of the middle image to 2:

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/sizing/lib/main.dart (expanded-images-with-flex)" replace="/flex.*/[!$&!]/g"?>
  {% prettify dart context="html" %}
  Row(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      Expanded(
        child: Image.asset('images/pic1.jpg'),
      ),
      Expanded(
        [!flex: 2,!]
        child: Image.asset('images/pic2.jpg'),
      ),
      Expanded(
        child: Image.asset('images/pic3.jpg'),
      ),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-expanded-visual.png class="mw-100"
      alt="Row of 3 images with the middle image twice as wide as the others" %}

  **App source:** [sizing]({{examples}}/layout/sizing)
</div>
</div>

[sizing]: {{examples}}/layout/sizing

### Packing widgets

By default, a row or column occupies as much space along its main axis
as possible, but if you want to pack the children closely together,
set its `mainAxisSize` to `MainAxisSize.min`. The following example
uses this property to pack the star icons together.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/pavlova/lib/main.dart (stars)" replace="/mainAxisSize.*/[!$&!]/g; /\w+ \w+ = //g; /;//g"?>
  {% prettify dart context="html" %}
  Row(
    [!mainAxisSize: MainAxisSize.min,!]
    children: [
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.black),
      Icon(Icons.star, color: Colors.black),
    ],
  )
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/packed.png class="border mw-100"
      alt="Row of 5 stars, packed together in the middle of the row" %}

  **App source:** [pavlova]({{examples}}/layout/pavlova)
</div>
</div>

### Nesting rows and columns

The layout framework allows you to nest rows and columns inside of rows
and columns as deeply as you need. Let's look the code for the outlined section
of the following layout:

{% asset ui/layout/pavlova-large-annotated.png class="border mw-100"
    alt="Screenshot of the pavlova app, with the ratings and icon rows outlined in red" %}
{:.text-center}

The outlined section is implemented as two rows. The ratings row contains
five stars and the number of reviews. The icons row contains three
columns of icons and text.

The widget tree for the ratings row:

{% asset ui/layout/widget-tree-pavlova-rating-row.png class="mw-100" alt="Ratings row widget tree" %}
{:.text-center}

The `ratings` variable creates a row containing a smaller row of 5 star icons,
and text:

<?code-excerpt "layout/pavlova/lib/main.dart (ratings)" replace="/ratings/[!$&!]/g"?>
```dart
var stars = Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.black),
    Icon(Icons.star, color: Colors.black),
  ],
);

final [!ratings!] = Container(
  padding: EdgeInsets.all(20),
  child: Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      stars,
      Text(
        '170 Reviews',
        style: TextStyle(
          color: Colors.black,
          fontWeight: FontWeight.w800,
          fontFamily: 'Roboto',
          letterSpacing: 0.5,
          fontSize: 20,
        ),
      ),
    ],
  ),
);
```

{{site.alert.tip}}
  To minimize the visual confusion that can result from heavily nested layout
  code, implement pieces of the UI in variables and functions.
{{site.alert.end}}

The icons row, below the ratings row, contains 3 columns; each column contains
an icon and two lines of text, as you can see in its widget tree:

{% asset ui/layout/widget-tree-pavlova-icon-row.png class="mw-100" alt="Icon widget tree" %}
{:.text-center}

The `iconList` variable defines the icons row:

<?code-excerpt "layout/pavlova/lib/main.dart (iconList)" replace="/iconList/[!$&!]/g"?>
```dart
final descTextStyle = TextStyle(
  color: Colors.black,
  fontWeight: FontWeight.w800,
  fontFamily: 'Roboto',
  letterSpacing: 0.5,
  fontSize: 18,
  height: 2,
);

// DefaultTextStyle.merge() allows you to create a default text
// style that is inherited by its child and all subsequent children.
final [!iconList!] = DefaultTextStyle.merge(
  style: descTextStyle,
  child: Container(
    padding: EdgeInsets.all(20),
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
      children: [
        Column(
          children: [
            Icon(Icons.kitchen, color: Colors.green[500]),
            Text('PREP:'),
            Text('25 min'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.timer, color: Colors.green[500]),
            Text('COOK:'),
            Text('1 hr'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.restaurant, color: Colors.green[500]),
            Text('FEEDS:'),
            Text('4-6'),
          ],
        ),
      ],
    ),
  ),
);
```

The `leftColumn` variable contains the ratings and icons rows, as well as the
title and text that describes the Pavlova:

<?code-excerpt "layout/pavlova/lib/main.dart (leftColumn)" replace="/leftColumn/[!$&!]/g"?>
```dart
final [!leftColumn!] = Container(
  padding: EdgeInsets.fromLTRB(20, 30, 20, 20),
  child: Column(
    children: [
      titleText,
      subTitle,
      ratings,
      iconList,
    ],
  ),
);
```

The left column is placed in a `Container` to constrain its width.
Finally, the UI is constructed with the entire row (containing the
left column and the image) inside a `Card`.

The [Pavlova image][] is from [Pixabay][].
You can embed an image from the net using `Image.network()` but,
for this example, the image is saved to an images directory in the project,
added to the [pubspec file,]({{examples}}/layout/pavlova/pubspec.yaml)
and accessed using `Images.asset()`. For more information, see
[Adding assets and images](/docs/development/ui/assets-and-images).

<?code-excerpt "layout/pavlova/lib/main.dart (body)"?>
```dart
body: Center(
  child: Container(
    margin: EdgeInsets.fromLTRB(0, 40, 0, 30),
    height: 600,
    child: Card(
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Container(
            width: 440,
            child: leftColumn,
          ),
          mainImage,
        ],
      ),
    ),
  ),
),
```

{{site.alert.tip}}
  The Pavlova example runs best horizontally on a wide device, such as a tablet.
  If you are running this example in the iOS simulator, you can select a
  different device using the **Hardware > Device** menu. For this example, we
  recommend the iPad Pro. You can change its orientation to landscape mode using
  **Hardware > Rotate**. You can also change the size of the simulator window
  (without changing the number of logical pixels) using **Window > Scale**.
{{site.alert.end}}

**App source:** [pavlova]({{examples}}/layout/pavlova)

[Pavlova image]: https://pixabay.com/en/photos/pavlova
[Pixabay]: https://pixabay.com/en/photos/pavlova

<hr>

## Common layout widgets

Flutter has a rich library of layout widgets. Here are a few of those most
commonly used. The intent is to get you up and running as quickly as possible,
rather than overwhelm you with a complete list.  For information on other
available widgets, refer to the [Widget catalog][],
or use the Search box in the [API reference docs](https://docs.flutter.io).
Also, the widget pages in the API docs often make suggestions
about similar widgets that might better suit your needs.

The following widgets fall into two categories: standard widgets from the
[widgets library][], and specialized widgets from the [Material library][]. Any
app can use the widgets library but only Material apps can use the Material
Components library.

### Standard widgets

* [Container](#container): Adds padding, margins, borders, background color, or
  other decorations to a widget.
* [GridView](#gridview): Lays widgets out as a scrollable grid.
* [ListView](#listview): Lays widgets out as a scrollable list.
* [Stack](#stack): Overlaps a widget on top of another.

### Material widgets

* [Card](#card): Organizes related info into a box with rounded corners and a
  drop shadow.
* [ListTile](#listtile): Organizes up to 3 lines of text, and optional leading
  and trailing icons, into a row.

### Container

Many layouts make liberal use of [Container][]s to separate widgets using padding,
or to add borders or margins. You can change the device's background by
placing the entire layout into a `Container` and changing its background color
or image.

<div class="row">
<div class="col-lg-6" markdown="1">
  <h4 class="no_toc">Summary (Container)</h4>

  * Add padding, margins, borders
  * Change background color or image
  * Contains a single child widget, but that child can be a Row, Column,
    or even the root of a widget tree
</div>
<div class="col-lg-6 text-center">
  {% asset ui/layout/margin-padding-border.png class="mb-4 mw-100"
      width="230px"
      alt="Diagram showing: margin, border, padding, and content" %}
</div>
</div>

#### Examples (Container)
{:.no_toc}

This layout consists of a column with two rows, each containing 2 images. A
[Container][] is used to change the background color of the column to a lighter
grey.

<div class="row">
<div class="col-lg-7">
  <?code-excerpt "layout/container/lib/main.dart (column)" replace="/\bContainer/[!$&!]/g;"?>
  {% prettify dart context="html" %}
  Widget _buildImageColumn() => [!Container!](
        decoration: BoxDecoration(
          color: Colors.black26,
        ),
        child: Column(
          children: [
            _buildImageRow(1),
            _buildImageRow(3),
          ],
        ),
      );
  {% endprettify %}
</div>
<div class="col-lg-5 text-center">
  {% asset ui/layout/container.png class="mb-4 mw-100" width="230px"
      alt="Screenshot showing 2 rows, each containing 2 images" %}
</div>
</div>

A `Container` is also used to add a rounded border and margins to each image:

<?code-excerpt "layout/container/lib/main.dart (row)" replace="/\bContainer/[!$&!]/g;"?>
```dart
Widget _buildDecoratedImage(int imageIndex) => Expanded(
      child: [!Container!](
        decoration: BoxDecoration(
          border: Border.all(width: 10, color: Colors.black38),
          borderRadius: const BorderRadius.all(const Radius.circular(8)),
        ),
        margin: const EdgeInsets.all(4),
        child: Image.asset('images/pic$imageIndex.jpg'),
      ),
    );

Widget _buildImageRow(int imageIndex) => Row(
      children: [
        _buildDecoratedImage(imageIndex),
        _buildDecoratedImage(imageIndex + 1),
      ],
    );
```

You can find more `Container` examples in the [tutorial][] and the [Flutter
Gallery][].

**App source:** [container]({{examples}}/layout/container)

<hr>

### GridView

Use [GridView][] to lay widgets out as a two-dimensional list. `GridView`
provides two pre-fabricated lists, or you can build your own custom grid. When a
`GridView` detects that its contents are too long to fit the render box, it
automatically scrolls.

#### Summary (GridView)
{:.no_toc}

* Lays widgets out in a grid
* Detects when the column content exceeds the render box and automatically
  provides scrolling
* Build your own custom grid, or use one of the provided grids:
  * `GridView.count` allows you to specify the number of columns
  * `GridView.extent` allows you to specify the maximum pixel width of a tile
{% comment %}
* Use `MediaQuery.of(context).orientation` to create a grid that changes
  its layout depending on whether the device is in landscape or portrait mode.
{% endcomment %}

{{site.alert.note}}
  When displaying a two-dimensional list where it's important which
  row and column a cell occupies (for example,
  it's the entry in the "calorie" column for the "avocado" row), use
  [Table]({{api}}/widgets/Table-class.html) or
  [DataTable]({{api}}/material/DataTable-class.html).
{{site.alert.end}}

#### Examples (GridView)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/gridview-extent.png class="mw-100" alt="A 3-column grid of photos" %}
  {:.text-center}

  Uses `GridView.extent` to create a grid with tiles a maximum 150 pixels wide.

  **App source:** [grid_and_list]({{examples}}/layout/grid_and_list)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/gridview-count-flutter-gallery.png class="mw-100"
      alt="A 2 column grid with footers" %}
  {:.text-center}

  Uses `GridView.count` to create a grid that's 2 tiles wide in portrait mode,
  and 3 tiles wide in landscape mode. The titles are created by setting the
  `footer` property for each [GridTile][].

  **Dart code:** [grid_list_demo.dart]({{demo}}/material/grid_list_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/grid_and_list/lib/main.dart (grid)" replace="/\GridView/[!$&!]/g;"?>
```dart
Widget _buildGrid() => [!GridView!].extent(
    maxCrossAxisExtent: 150,
    padding: const EdgeInsets.all(4),
    mainAxisSpacing: 4,
    crossAxisSpacing: 4,
    children: _buildGridTileList(30));

// The images are saved with names pic0.jpg, pic1.jpg...pic29.jpg.
// The List.generate() constructor allows an easy way to create
// a list when objects have a predictable naming pattern.
List<Container> _buildGridTileList(int count) => List.generate(
    count, (i) => Container(child: Image.asset('images/pic$i.jpg')));
```

<hr>

### ListView

[ListView]({{api}}/widgets/ListView-class.html),
a column-like widget, automatically provides scrolling when
its content is too long for its render box.

#### Summary (ListView)
{:.no_toc}

* A specialized [Column][] for organizing a list of boxes
* Can be laid out horizontally or vertically
* Detects when its content won't fit and provides scrolling
* Less configurable than `Column`, but easier to use and supports scrolling

#### Examples (ListView)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listview.png class="border mw-100"
      alt="ListView containing movie theaters and restaurants" %}
  {:.text-center}

  Uses `ListView` to display a list of businesses using `ListTile`s. A `Divider`
  separates the theaters from the restaurants.

  **App source:** [grid_and_list]({{examples}}/layout/grid_and_list)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listview-flutter-gallery.png class="border mw-100"
      alt="ListView containing shades of blue" %}
  {:.text-center}

  Uses `ListView` to display the [Colors]({{api}}/material/Colors-class.html) from
  the [Material Design palette](https://material.io/guidelines/style/color.html)
  for a particular color family.

  **Dart code:** [colors_demo.dart]({{demo}}/colors_demo.dart) from the
  [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/grid_and_list/lib/main.dart (list)" replace="/\ListView/[!$&!]/g;"?>
```dart
Widget _buildList() => [!ListView!](
      children: [
        _tile('CineArts at the Empire', '85 W Portal Ave', Icons.theaters),
        _tile('The Castro Theater', '429 Castro St', Icons.theaters),
        _tile('Alamo Drafthouse Cinema', '2550 Mission St', Icons.theaters),
        _tile('Roxie Theater', '3117 16th St', Icons.theaters),
        _tile('United Artists Stonestown Twin', '501 Buckingham Way',
            Icons.theaters),
        _tile('AMC Metreon 16', '135 4th St #3000', Icons.theaters),
        Divider(),
        _tile('K\'s Kitchen', '757 Monterey Blvd', Icons.restaurant),
        _tile('Emmy\'s Restaurant', '1923 Ocean Ave', Icons.restaurant),
        _tile(
            'Chaiya Thai Restaurant', '272 Claremont Blvd', Icons.restaurant),
        _tile('La Ciccia', '291 30th St', Icons.restaurant),
      ],
    );

ListTile _tile(String title, String subtitle, IconData icon) => ListTile(
      title: Text(title,
          style: TextStyle(
            fontWeight: FontWeight.w500,
            fontSize: 20,
          )),
      subtitle: Text(subtitle),
      leading: Icon(
        icon,
        color: Colors.blue[500],
      ),
    );
```

<hr>

### Stack

Use [Stack][] to arrange widgets on top of a base widget&mdash;often an image.
The widgets can completely or partially overlap the base widget.

#### Summary (Stack)
{:.no_toc}

* Use for widgets that overlap another widget
* The first widget in the list of children is the base widget;
  subsequent children are overlaid on top of that base widget
* A `Stack`'s content can't scroll
* You can choose to clip children that exceed the render box

#### Examples (Stack)
{:.no_toc}

<div class="row">
<div class="col-lg-7" markdown="1">
  {% asset ui/layout/stack.png class="mw-100" width="200px" alt="Circular avatar image with a label" %}
  {:.text-center}

  Uses `Stack` to overlay a `Container` (that displays its `Text` on a translucent
  black background) on top of a `CircleAvatar`.
  The `Stack` offsets the text using the `alignment` property and
  `Alignment`s.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-5" markdown="1">
  {% asset ui/layout/stack-flutter-gallery.png class="mw-100" alt="An image with a grey gradient across the top" %}
  {:.text-center}

  Uses `Stack` to overlay a gradient to the top of the image. The gradient
  ensures that the toolbar's icons are distinct against the image.

  **Dart code:** [contacts_demo.dart]({{demo}}/contacts_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/card_and_stack/lib/main.dart (Stack)" replace="/\bStack/[!$&!]/g;"?>
```dart
Widget _buildStack() => [!Stack!](
    alignment: const Alignment(0.6, 0.6),
    children: [
      CircleAvatar(
        backgroundImage: AssetImage('images/pic.jpg'),
        radius: 100,
      ),
      Container(
        decoration: BoxDecoration(
          color: Colors.black45,
        ),
        child: Text(
          'Mia B',
          style: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.bold,
            color: Colors.white,
          ),
        ),
      ),
    ],
  );
```

<hr>

### Card

A [Card][], from the [Material library][], contains related nuggets of
information and can be composed from almost any widget, but is often used with
[ListTile][]. `Card` has a single child, but its child can be a column, row,
list, grid, or other widget that supports multiple children. By default, a
`Card` shrinks its size to 0 by 0 pixels. You can use [SizedBox][] to constrain
the size of a card.

In Flutter, a `Card` features slightly rounded corners and a drop shadow, giving
it a 3D effect. Changing a `Card`'s `elevation` property allows you to control
the drop shadow effect. Setting the elevation to 24, for example, visually lifts
the `Card` further from the surface and causes the shadow to become more
dispersed. For a list of supported elevation values, see [Elevation][] in the
[Material guidelines][Material Design]. Specifying an unsupported value disables
the drop shadow entirely.

#### Summary (Card)
{:.no_toc}

* Implements a [Material card][]
* Used for presenting related nuggets of information
* Accepts a single child, but that child can be a `Row`, `Column`, or other
  widget that holds a list of children
* Displayed with rounded corners and a drop shadow
* A `Card`'s content can't scroll
* From the [Material library][]

#### Examples (Card)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card.png class="mw-100" alt="Card containing 3 ListTiles" %}
  {:.text-center}

  A `Card` containing 3 ListTiles and sized by wrapping it with a `SizedBox`. A
  `Divider` separates the first and second `ListTiles`.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card-flutter-gallery.png class="mw-100"
      alt="Card containing an image, text and buttons" %}
  {:.text-center}

  A `Card` containing an image and text.

  **Dart code:** [cards_demo.dart]({{demo}}/material/cards_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/card_and_stack/lib/main.dart (Card)" replace="/\bCard/[!$&!]/g;"?>
```dart
Widget _buildCard() => SizedBox(
    height: 210,
    child: [!Card!](
      child: Column(
        children: [
          ListTile(
            title: Text('1625 Main Street',
                style: TextStyle(fontWeight: FontWeight.w500)),
            subtitle: Text('My City, CA 99984'),
            leading: Icon(
              Icons.restaurant_menu,
              color: Colors.blue[500],
            ),
          ),
          Divider(),
          ListTile(
            title: Text('(408) 555-1212',
                style: TextStyle(fontWeight: FontWeight.w500)),
            leading: Icon(
              Icons.contact_phone,
              color: Colors.blue[500],
            ),
          ),
          ListTile(
            title: Text('costa@example.com'),
            leading: Icon(
              Icons.contact_mail,
              color: Colors.blue[500],
            ),
          ),
        ],
      ),
    ),
  );
```
<hr>

### ListTile

Use [ListTile][], a specialized row widget from the [Material library][], for an
easy way to create a row containing up to 3 lines of text and optional leading
and trailing icons. `ListTile` is most commonly used in [Card][] or
[ListView][], but can be used elsewhere.

#### Summary (ListTile)
{:.no_toc}

* A specialized row that contains up to 3 lines of text and optional icons
* Less configurable than `Row`, but easier to use
* From the [Material library][]

#### Examples (ListTile)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card.png class="mw-100" alt="Card containing 3 ListTiles" %}
  {:.text-center}

  A `Card` containing 3 `ListTiles`.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listtile-flutter-gallery.png class="border mw-100" height="200px"
      alt="3 ListTiles, each containing a pull-down button" %}
  {:.text-center}

  Uses `ListTile` to list 3 drop down button types.<br>
  **Dart code:** [buttons_demo.dart]({{demo}}/material/buttons_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<hr>

## Resources

The following resources may help when writing layout code.

* [Layout tutorial](/docs/development/ui/layout/tutorial)
: Learn how to build a layout.
* [Widget Overview](/docs/development/ui/widgets)
: Describes many of the widgets available in Flutter.
* [HTML/CSS Analogs in Flutter](/docs/get-started/flutter-for/web-devs)
: For those familiar with web programming, this page maps HTML/CSS functionality
  to Flutter features.
* [Flutter Gallery][]
: Demo app showcasing many Material Design widgets and other Flutter features.
* [Flutter API documentation](https://docs.flutter.io/)
: Reference documentation for all of the Flutter libraries.
* [Dealing with Box Constraints in Flutter](/docs/development/ui/layout/box-constraints)
: Discusses how widgets are constrained by their render boxes.
* [Adding Assets and Images in Flutter](/docs/development/ui/assets-and-images)
: Explains how to add images and other assets to your app's package.
* [Zero to One with Flutter](https://medium.com/@mravn/zero-to-one-with-flutter-43b13fd7b354)
: One person's experience writing his first Flutter app.

[build()]: {{api}}/widgets/StatelessWidget/build.html
[Card]: {{api}}/material/Card-class.html
[Center]: {{api}}/widgets/Center-class.html
[Column]: {{api}}/widgets/Column-class.html
[Container]: {{api}}/widgets/Container-class.html
[Elevation]: https://material.io/design/environment/elevation.html
[Expanded]: {{api}}/widgets/Expanded-class.html
[Flutter Gallery]: {{site.repo.flutter}}/tree/master/examples/flutter_gallery
[GridView]: {{api}}/widgets/GridView-class.html
[GridTile]: {{api}}/material/GridTile-class.html
[Icon]: {{api}}/material/Icons-class.html
[Image]: {{api}}/widgets/Image-class.html
[layout widgets]: /docs/development/ui/widgets/layout
[ListTile]: {{api}}/material/ListTile-class.html
[ListView]: {{api}}/widgets/ListView-class.html
[Material card]: https://material.io/design/components/cards.html
[Material Design]: https://material.io/design
[Material library]: {{api}}/material/material-library.html
[Row]: {{api}}/widgets/Row-class.html
[Scaffold]: {{api}}/material/Scaffold-class.html
[SizedBox]: {{api}}/widgets/SizedBox-class.html
[Stack]: {{api}}/widgets/Stack-class.html
[Text]: {{api}}/widgets/Text-class.html
[tutorial]: /docs/development/ui/layout/tutorial
[widgets library]: {{api}}/widgets/widgets-library.html
[Widget catalog]: /docs/development/ui/widgets
