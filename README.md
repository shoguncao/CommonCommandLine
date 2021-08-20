#### 获取模拟器沙盒目录  
* ```xcrun simctl get_app_container booted "com.tencent.tgatk" data```  
> 其中"com.tencent.tgatk"换为自己构建的app的Bundle Identifier

#### 查看Mach-O是否有某个函数
* ```otool -ov /Users/shoguncao/Library/Developer/Xcode/DerivedData/example-gpzqsrogwlcrekbqpowhbsjfhxea/Build/Products/Debug-iphonesimulator/example.app/example | grep 'myTest'```  
* ```nm -nm /Users/shoguncao/Library/Developer/Xcode/DerivedData/example-gpzqsrogwlcrekbqpowhbsjfhxea/Build/Products/Debug-iphonesimulator/example.app/example | grep 'myTest'``` 

#### 将OC文件转换为C++
* ```xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m```

#### 对指定目录下所有git执行fetch
* ```find . -type d -print | grep '.git$' | awk -F '/.git' '{print $1}' | xargs -I dir sh -c "pushd dir; git fetch --all; popd;"```

#### 用symbolicatecrash解析iOS堆栈
* export需要的变量  
```export DEVELOPER_DIR=$(xcode-select --print-path)```  
* 查找symbolicatecrash路径  
```find /Applications/Xcode.app -name symbolicatecrash -type f```  
* 解析crash log
```/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash -v TGASDKDebugApp.ips TGASDKDebugApp.app.dSYM```

#### 用atos解析iOS堆栈  
* ```atos -o /Users/shoguncao/Downloads/TGASDKDebugApp.app.dSYM/Contents/Resources/DWARF/TGASDKDebugApp -l 0x1006b8000 0x10112a4ac```  

#### 通过file+grep+awk将git下所有mach-O文件加入lfs track  
* ```find . -type f | grep -v '/.git/' | xargs file | grep 'current ar archive' | awk -F '[ :]' '{print $1}' | uniq | xargs git lfs track```  

#### 通过perl -pi -e 替换指定字符串  
```
export old_header='#import "TGAAccountInfo.h"'                               
export new_header='#import <XXX/TGAAccountInfo.h>'                           
find . -type f | grep '.m$' | xargs perl -pi -e 's/$ENV{old_header}/$ENV{new_header}/g'
```