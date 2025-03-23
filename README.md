#### 获取模拟器沙盒目录  
* ```xcrun simctl get_app_container booted "com.tencent.tgatk" data```  
> 其中"com.tencent.tgatk"换为自己构建的app的Bundle Identifier

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

#### 用CrashSymbolicator.py解析iOS堆栈
* 查找CrashSymbolicator.py路径
```find /Applications/Xcode.app -name CrashSymbolicator.py -type f```
* 解析crash log
```python3 /Applications/Xcode.app/Contents/SharedFrameworks/CoreSymbolicationDT.framework/Versions/A/Resources/CrashSymbolicator.py -d /Users/shoguncao/Downloads/TTNetworkManager/TTNetworkManager.framework.dSYM /Users/shoguncao/Downloads/LetsGoClient-2025-01-07-121740.ips```

#### 用atos解析iOS堆栈  
* ```atos -o /Users/shoguncao/Downloads/TGASDKDebugApp.app.dSYM/Contents/Resources/DWARF/TGASDKDebugApp -l 0x1006b8000 0x10112a4ac```  

#### 查看cer文件
`openssl x509 -inform der -in demo.cer`

#### 查看p12文件
`openssl pkcs12 -in 证书.p12 -out 证书.pem -nodes`

#### 查看CertSigningRequest文件
`openssl req -noout -text -in xxx.certSigningRequest -pubkey`

#### 查看pem文件
`openssl x509 -in xxx.pem -noout -pubkey`

#### 私钥转公钥  
```
openssl pkcs12 -in private.p12 -nodes -out private.pem
openssl rsa -in private.pem -pubout > public.pem
```

#### 从p12秘钥请求CSR(CertSigningRequest)文件
```
openssl pkcs12 -in private.p12 -nodes -out private.pem
openssl req -newkey rsa:2048 -key private.pem -out private.csr
```

#### 查看mach-O文件符号
* 查看静态库`objdump liba.a --syms`
* 查看动态库`/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/dyldinfo -export liba.dylib`

#### 由CMakeLists.txt生成Xcode工程
```
cmake .. -G Xcode \
-DCMAKE_TOOLCHAIN_FILE=cmake/ios.toolchain.cmake \
-DPLATFORM=OS64 -DARCHS="arm64" -DCMAKE_SYSTEM_PROCESSOR=arm64 \
-DENABLE_BITCODE=0 -DENABLE_ARC=0 -DENABLE_VISIBILITY=1 -DDEPLOYMENT_TARGET=9.3 \
-DDynamicBinaryInstrument=ON -DNearBranch=ON -DPlugin.SymbolResolver=ON -DPlugin.Darwin.HideLibrary=ON -DPlugin.Darwin.ObjectiveC=ON
```

#### 开启/停止XCode的Indexing  
- 停止  
`defaults write com.apple.dt.XCode IDEIndexDisable 1`  
- 开启  
`defaults write com.apple.dt.XCode IDEIndexDisable 0`

#### protobuf生成swift命令  
`protoc --swift_out=. chatmsg.proto --swift_opt=Visibility=Public`  

#### mac上无法打开xxx，因为无法验证开发者
`sudo spctl --master-disable`  

#### iPhone手机连接Mac后取控制台日志命令
`sudo log collect --device --start '2023-06-01 00:00:00' --output /Users/shoguncao/Downloads`

#### UE通过命令行编译.uproject
`/Users/Shared/Epic\ Games/UE_4.26/Engine/Build/BatchFiles/Mac/Build.sh CapturerDemo iOS Development -Project="/Users/shoguncao/work/codes/ESTV/UE_Capturer_Demo/CapturerDemo.uproject" -WaitMutex`

#### UE通过命令行生成Xcode工程
`/Users/Shared/Epic\ Games/UE_4.26/Engine/Build/BatchFiles/Mac/GenerateProjectFiles.sh -project="/Users/shoguncao/work/codes/ESTV/UE_Capturer_Demo/CapturerDemo.uproject" -game`  

#### Unity通过命令行编译
/Applications/Unity/Hub/Editor/5.6.4p4/Unity.app/Contents/MacOS/Unity -batchmode -nographics -projectPath /Users/shoguncao/work/codes/ESTV/unity_box_record_cf_compat -executeMethod BuildPlayerExample.MyBuild  -logFile /Users/shoguncao/Downloads/11.log

#### demangle符号
- *c/c++/objective-c*:  
```
c++filt __ZN4mars4xlog15XloggerAppender11NewInstanceERKNS0_10XLogConfigEy
```
- *swift*:
```
swift demangle __swift_FORCE_LOAD_$_swiftCoreFoundation
```