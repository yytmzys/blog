# [SwiftUI](https://github.com/yytmzys/blog/issues/101)

### 2 List, 一个容器，显示排列在单个列中的数据行。类似于 UIKit 中 UITableView 的效果
#### 创建静态可滚动 列表
```
List {
        Text("Hello world")
        Text("Hello world")
        Text("Hello world")
      }
```
![截屏2023-06-15 00.01.09.png](https://upload-images.jianshu.io/upload_images/6357009-86bb6bf200fd8769.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 混合排版
```
List {
        Text("Hello world")
        Image(systemName: "clock")
      }
```
![截屏2023-06-15 00.02.24.png](https://upload-images.jianshu.io/upload_images/6357009-0e88af56e2dbfb71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 创建动态列表
```
let names = ["John", "Apple", "Seed"]
      List(names, id: \.self) { name in
        Text(name)
      }
```
![截屏2023-06-19 00.06.06.png](https://upload-images.jianshu.io/upload_images/6357009-835676e1cdd3fce4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 添加 section
```
List {
        Section(header: Text("UIKit"), footer: Text("We will miss you")) {
          Text("UITableView")
        }

        Section(header: Text("SwiftUI"), footer: Text("A lot to learn")) {
          Text("List")
        }
      }
```
![截屏2023-06-19 20.19.47.png](https://upload-images.jianshu.io/upload_images/6357009-3c30bb730f1f3c6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 要使其分组添加 .listStyle(GroupedListStyle()), 要使其插入分组 (.insetGrouped)，请添加 .listStyle(GroupedListStyle()) ,  ??? 并强制常规水平尺寸类 .environment(\.horizontalSizeClass, .regular)。
```
List {
        Section(header: Text("UIKit"), footer: Text("We will miss you")) {
          Text("UITableView")
        }

        Section(header: Text("SwiftUI"), footer: Text("A lot to learn")) {
          Text("List")
        }
      }
      .listStyle(.grouped )
      .environment(\.horizontalSizeClass, .regular)
```
![截屏2023-06-19 20.23.06.png](https://upload-images.jianshu.io/upload_images/6357009-c51ae252b1dde01a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




### 1 ScrollView, 滚动包装内容视图。滑动方向设置 ScrollView(.horizontal / .vertical)  或者 ScrollView([.horizontal, .vertical]) 多选
```
ScrollView {
        Image(systemName: "swift")
        Text("Hello World")
      }
```
![截屏2023-06-19 20.48.46.png](https://upload-images.jianshu.io/upload_images/6357009-f2237164309effea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.1 要使用 ScrollView，您需要放置一个 content 视图 您想要使其可滚动作为滚动视图内容。
```
ScrollView {
        VStack {
          ForEach(0..<100) { i in
            Text("Item \(i)")
          }
        }.frame(maxWidth: .infinity)
        
      }
```
![Simulator Screenshot - iPhone 8 - 2023-06-19 at 20.51.53.png](https://upload-images.jianshu.io/upload_images/6357009-d0f1e74a59cd656e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### LazyHGrid, 将其子视图排列在水平增长的网格中的容器视图，仅根据需要创建项目。
```
 var rows: [GridItem] =
              Array(repeating: .init(.fixed(20)), count: 2)

      ScrollView(.horizontal) {
        LazyHGrid(rows: rows, alignment: .top) {
          ForEach((0...100), id: \.self) {
            Text("\($0)").background(Color.pink)
          }
        }
        
      }
```
![截屏2023-06-19 21.10.19.png](https://upload-images.jianshu.io/upload_images/6357009-a35ef31afe037725.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### LazyVGrid, 将其子视图排列在垂直增长的网格中的容器视图，仅根据需要创建项目
```
var columns: [GridItem] =
              Array(repeating: .init(.fixed(20)), count: 5)

      ScrollView {
        LazyVGrid(columns: columns) {
          ForEach((0...100), id: \.self) {
            Text("\($0)").background(Color.pink)
          }
        }
        
      }
```
![Simulator Screenshot - iPhone 8 - 2023-06-19 at 21.20.31.png](https://upload-images.jianshu.io/upload_images/6357009-1fb827a3ae846af7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Form, 用于对用于数据输入的控件进行分组的容器，例如在设置或检查器中。您可以将几乎任何东西放入这个 Form 中，它会呈现适合表单的样式。
```
@State private var date = Date()
  @State private var selection: Int = 2
  
  var body: some View {
    VStack {
      
      
      
      NavigationView {
        Form {
          Section {
            Text("Plain Text")
            Stepper(value: $quantity, in: 0...10, label: { Text("Quantity") })
          }
          
          Section {
            DatePicker(selection: $date, label: { Text("Due Date") })
            Picker(selection: $selection, label:
                    Text("Picker Name")
                   , content: {
              Text("Value 1").tag(0)
              Text("Value 2").tag(1)
              Text("Value 3").tag(2)
              Text("Value 4").tag(3)
            })
          }
        }
        
      }
```
![Simulator Screenshot - iPhone 8 - 2023-06-19 at 21.28.52.png](https://upload-images.jianshu.io/upload_images/6357009-6f4754b4badacaf4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Spacer, 沿其包含堆栈布局的主轴扩展的灵活空间，或如果不包含在堆栈中，则在两个轴上。
```
HStack {
      Image(systemName: "clock")
      Spacer()
      Text("Time")
    }
    
    VStack {
      Image(systemName: "clock")
      Spacer()
      Text("Time")
      
    }
```
![Simulator Screenshot - iPhone 8 - 2023-06-19 at 21.33.39.png](https://upload-images.jianshu.io/upload_images/6357009-6e3f2eb0325b21c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Divider, 分割线, 可用于分隔其他内容的视觉元素。
```
HStack {
    Image(systemName: "clock")
    Divider()
    Text("Time")
}.fixedSize()
```
![截屏2023-06-19 21.35.20.png](https://upload-images.jianshu.io/upload_images/6357009-f388517d3175c033.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### NavigationView, 一个视图，用于显示表示导航中可见路径的视图堆栈层次结构。largeTitle
```
NavigationView {
        List {
            Text("Hello World")
        }
        .navigationBarTitle(Text("Navigation Title")) // Default to large title style
    }.foregroundColor(.red
```
![截屏2023-06-19 21.38.11.png](https://upload-images.jianshu.io/upload_images/6357009-142e535049e32388.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 旧式标题, 
```
NavigationView {
    List {
        Text("Hello World")
    }
    .navigationBarTitle(Text("Navigation Title"), displayMode: .inline)
}
```
![截屏2023-06-19 21.39.17.png](https://upload-images.jianshu.io/upload_images/6357009-ff71d9a9318ee163.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 添加UIBarButtonItem. 
```
NavigationView {
    List {
        Text("Hello World")
    }
    .navigationBarItems(trailing:
        Button(action: {
            // Add action
        }, label: {
            Text("Add")
        })
    )
    .navigationBarTitle(Text("Navigation Title"))
}
```
![截屏2023-06-19 21.41.16.png](https://upload-images.jianshu.io/upload_images/6357009-70a2e86bcff07f03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 添加 `show`/`push` 和 导航链接, 用于 `UISplitViewController`。
```
NavigationView {
    List {
        NavigationLink("Go to detail", destination: Text("New Detail"))
    }.navigationBarTitle("Master")
    Text("Placeholder for Detail")
}
```
![截屏2023-06-19 21.43.06.png](https://upload-images.jianshu.io/upload_images/6357009-6f7b16a53a337096.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 您可以使用两个新的样式属性设置 NavigationView 的样式：stack 和 doubleColumn。 默认情况下，iPhone 和 Apple TV 上的导航视图直观地反映导航堆栈，而在 iPad 和 Mac 上，拆分视图样式导航视图显示。

您可以使用 .navigationViewStyle 覆盖它。
```
NavigationView {
        List {
            Text("Hello World")
        }
        .navigationBarItems(trailing:
            Button(action: {
                // Add action
            }, label: {
                Text("Add")
            })
        )
        .navigationBarTitle(Text("Navigation Title"))
    }
    .navigationViewStyle(.stack)
```
![截屏2023-06-19 21.48.23.png](https://upload-images.jianshu.io/upload_images/6357009-cf4af74920e6688d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 在 iOS 14 中，UISplitViewController 新增了侧边栏样式。 你也可以通过在 NavigationView 下放置三个视图来做到这一点。
```
NavigationView {
    Text("Sidebar")
    Text("Primary")
    Text("Detail")
}.navigationSplitViewStyle(.prominentDetail
```
##### detail
![截屏2023-06-19 22.04.48.png](https://upload-images.jianshu.io/upload_images/6357009-0b102da9b6a39ad1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 在toolbarItems中添加UIToolbar UIViewController。
```
NavigationView {
      Text("SwiftUI").padding()
        .toolbar {
          ToolbarItem(placement: .bottomBar) {
            Button {
              
            } label: {
              Image(systemName: "archivebox")
            }
          }
          
          ToolbarItem(placement: .bottomBar) {
            Spacer()
          }
          
          ToolbarItem(placement: .bottomBar) {
            Button {
              
            } label: {
              Image(systemName: "square.and.pencil")
            }
          }
        }
      
    }
```
![截屏2023-06-19 22.09.39.png](https://upload-images.jianshu.io/upload_images/6357009-99a31005151fefbe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)