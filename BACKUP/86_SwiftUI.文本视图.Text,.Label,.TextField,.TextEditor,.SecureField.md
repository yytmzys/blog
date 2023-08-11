# [SwiftUI 文本视图 Text, Label, TextField, TextEditor, SecureField](https://github.com/yytmzys/blog/issues/86)

文本视图 Text, Label, TextField, TextEditor, SecureField, Image
------
### 1 Text 
#### 1.1 显示一行或多行只读文本的视图。
```Swift
Text("Hello World")
```
![截屏2023-06-13 21.15.22.png](https://upload-images.jianshu.io/upload_images/6357009-5b3ed68968f1c252.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 1.2 给文本添加 样式, 粗体 斜体 下划线,
```Swift
Text("Hello World")
  .bold()
  .italic()
  .underline()
  .lineLimit(2)
```
![截屏2023-06-13 21.20.10.png](https://upload-images.jianshu.io/upload_images/6357009-22296aa07a2f190e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 1.3 Text 中提供的字符串也用作LocalizedStringKey，所以您可以自由获得 NSLocalizedString 的行为。
![截屏2023-06-13 21.52.22.png](https://upload-images.jianshu.io/upload_images/6357009-7674aa3862f5b9cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 1.4 在文本视图中格式化文本。 实际上这不是 SwiftUI 功能，而是 Swift 5 字符串插值。
```Swift
static let dateFormatter: DateFormatter = {
      let formatter = DateFormatter()
      formatter.dateStyle = .long
      return formatter
  }()
  
  var now = Date()
  
  var body: some View {
    VStack {
      Text("What time is it?: \(now, formatter: Self.dateFormatter)")
    }
    .padding()
  }
```
![截屏2023-06-13 21.52.22.png](https://upload-images.jianshu.io/upload_images/6357009-94bb69d30bbd3efe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 1.5 将 Text 与 + 连接在一起。
```Swift
Text("Hello ") + Text("World!").bold() // 粗体
```
![截屏2023-06-13 21.55.30.png](https://upload-images.jianshu.io/upload_images/6357009-5ba2e61ab2acb815.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 1.6 文本对齐
```Swift
Text("Hello\nWorld!").multilineTextAlignment(.center)
```
![截屏2023-06-13 21.58.18.png](https://upload-images.jianshu.io/upload_images/6357009-560fc7127720caac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2 Label 
#### 2.1 标签 是一种方便的视图，可以将图像和文本并排显示。 适用于菜单项或设置。
您可以使用自己的图像或 SF Symbol。
```Swift
Label("Swift", image: "swift")
Label("Website", systemImage: "globe")
```
![截屏2023-06-13 22.08.44.png](https://upload-images.jianshu.io/upload_images/6357009-6b497653d8f2167b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3 TextField. 显示可编辑文本界面的控件。
```Swift
@State var name: String = "John"
var body: some View {
    TextField("Name's placeholder", text: $name)
        .textFieldStyle(RoundedBorderTextFieldStyle())
        .padding()
}
```
![截屏2023-06-13 22.13.09.png](https://upload-images.jianshu.io/upload_images/6357009-b6a5f4e4bedf78ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4 SecureField, 用户可以安全地输入密码或者私有文本的控件。
```Swift
@State var password: String = "1234"
var body: some View {
    SecureField("密码", text: $password)
        .textFieldStyle(RoundedBorderTextFieldStyle())
        .padding()
}
```
![截屏2023-06-13 22.21.50.png](https://upload-images.jianshu.io/upload_images/6357009-9d5804654f47e78f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 5 TextEditor, 一个可以显示和编辑长文本的视图。
```Swift
@State private var fullText: String = "This is some editable text..."

var body: some View {
    TextEditor(text: $fullText)
}
```
![截屏2023-06-13 22.16.20.png](https://upload-images.jianshu.io/upload_images/6357009-4aa4a09b62a1bdb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 7 Image, 显示环境相关图像的视图。
```Swift
Image(systemName: "globe")
```
![截屏2023-06-13 22.39.46.png](https://upload-images.jianshu.io/upload_images/6357009-5f997aa218259cfd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 7.1 我们可以使用新的 SF 符号, 可以为系统图标集添加样式以匹配您使用的字体
```Swift
Image(systemName: "clock.fill")      
      Image(systemName: "cloud.heavyrain.fill")
       .foregroundColor(.red)
       .font(.title)
      Image(systemName: "clock")
       .foregroundColor(.red)
       .font(Font.system(.largeTitle).bold())
```
![截屏2023-06-13 22.41.30.png](https://upload-images.jianshu.io/upload_images/6357009-c86a14052dc69a82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 7.2 给图片添加样式
```Swift
Image(systemName: "clock.fill")
        .resizable() // it will sized so that it fills all the available space
        .aspectRatio(contentMode: .fit)
```
![截屏2023-06-13 22.44.26.png](https://upload-images.jianshu.io/upload_images/6357009-97de33c6a49ad9bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)