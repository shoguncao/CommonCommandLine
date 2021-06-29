#### 获取模拟器沙盒目录  
* xcrun simctl get_app_container booted "com.tencent.tgatk" data  
> 其中"com.tencent.tgatk"换为自己构建的app的Bundle Identifier

#### 查看Mach-O是否有某个函数
* otool -ov /Users/shoguncao/Library/Developer/Xcode/DerivedData/example-gpzqsrogwlcrekbqpowhbsjfhxea/Build/Products/Debug-iphonesimulator/example.app/example | grep 'myTest'  
* nm -nm /Users/shoguncao/Library/Developer/Xcode/DerivedData/example-gpzqsrogwlcrekbqpowhbsjfhxea/Build/Products/Debug-iphonesimulator/example.app/example | grep 'myTest' 