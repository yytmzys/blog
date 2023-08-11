# [SwiftUI 视图和布局 HStack VStack ZStack](https://github.com/yytmzys/blog/issues/99)

### 1 HStack, 将子视图横向 水平 排列。

#### 1.1 创建文本 横向 展示
```
HStack (alignment: .center, spacing: 20){
        Text("Hello")
        Divider()
        Text("World")
      }
```
![截屏2023-06-14 23.45.27.png](https://upload-images.jianshu.io/upload_images/6357009-37ceac79e2dff28c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.2 LazyHStack, 创建滑动列表, 横向 水平 展示, 排列成一条水平增长的线的视图。
```
ScrollView(.horizontal) {
    LazyHStack(alignment: .center, spacing: 20) {
        ForEach(1...100, id: \.self) {
            Text("Column \($0)")
        }
    }
}
```
![截屏2023-06-14 23.49.31.png](https://upload-images.jianshu.io/upload_images/6357009-f35e13d4fd45e397.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2 VStack, 将其子级排列成一条垂直线的视图。
#### 2.1 创建 纵向 竖直 可滚动列表
```
VStack (alignment: .center, spacing: 20){
          Text("Hello")
          Divider()
          Text("World")
      }
```
![截屏2023-06-14 23.54.04.png](https://upload-images.jianshu.io/upload_images/6357009-0ffca2b3e0f09eae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.2 LazyVStack, 将其子项排列成一条垂直排列的视图，创建项目仅在需要时。
```
ScrollView(.vertical) {
          LazyVStack(alignment: .center, spacing: 20) {
              ForEach(1...100, id: \.self) {
                  Text("Column \($0)")
              }
          }
      }
```
![截屏2023-06-14 23.56.41.png](https://upload-images.jianshu.io/upload_images/6357009-704ed4f272d6c61b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3 ZStack, 重叠 覆盖 其子级的视图，使它们在两个轴上对齐。
```
ZStack {
        Text("Hello")
          .padding(10)
          .background(Color.red)
          .opacity(0.8)
        Text("World")
          .padding(20)
          .background(Color.red)
          .offset(x: 0, y: 40)
      }
```
![截屏2023-06-14 23.58.36.png](https://upload-images.jianshu.io/upload_images/6357009-bec03791ce4b68cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### LazyHGrid, 将其子视图排列在水平增长的网格中的容器视图，仅根据需要创建项目。


### 4 混合排版
```
        Text("Hello world")
        Image(systemName: "clock")      
```
![截屏2023-06-19 20.01.02.png](https://upload-images.jianshu.io/upload_images/6357009-06053477133a9a8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
