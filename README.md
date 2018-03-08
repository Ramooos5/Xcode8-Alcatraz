# Xcode8-Alcatraz

### Xcode8下使用插件Alcatraz

* #### 前言

  由于Xcode8不再支持使用Alcatraz管理插件,插件因为没有签名而导致无法打包等多种问题,所以我们采用双Xcode的方式来使用插件.其中原版的Xcode用来打包,编辑版Xcode用来日常开发.

* #### 步骤

  * ##### 复制原版xcode,并使用复制版本安装插件

    1. 从Appstore下载最新版的Xcode8.

    2. 如果想保留当前的Xcode7版本,先将xcode7重命名,这样下载的Xcode8不会覆盖Xcode7.

    3. 通过 [Finder] — [应用程序] 找到Xcode8, 复制一份,并重命名为 Xcode_Coding,用于安装插件和开发.

    4. 在Terminal中使用命令

       ```
       sudo xcode-select -s /Applications/Xcode_Coding.app
       ```

       来手动设置支持哪个版本的Xcode来编译代码.这个要和当前开发使用的Xcode版本对应.

    5. 使用另一个未被支持的版本开发程序,会导致程序在模拟器上无法运行,但是可以打包.

  * ##### 为复制版本的Xcode_Coding签名

    1. 打开电脑中钥匙串 

       这里我使用快捷搜索 Control + Space调用搜索框,再搜索keychain Access.app访问钥匙串

       ![](https://ws1.sinaimg.cn/large/006tNc79gy1fi5j3s8tdtj311e0780uy.jpg)

    2. [菜单栏] — [钥匙串访问] — [证书助理] — [创建证书]

       [menu] — [keychain Access] — [Certificate Assistant] — [Create Certificate]

       ![](https://ws2.sinaimg.cn/large/006tNc79gy1fi5j3trrc1j30pa0emtf4.jpg)

    3. 填写创建证书信息

       名称: Xcode_Coding_Signer

       身份类型:自签名根证书(self signed root)

       证书类型:代码签名(code signing)

       ![](https://ws4.sinaimg.cn/large/006tNc79gy1fi5j3vix8kj30y00nygpa.jpg)

    4. 在Terminal中通过新创建的证书为 XCode_Coding版本签名

       ```
       sudo codesign -f -s Xcode_Coding_Signer /Applications/Xcode_Coding.app
       ```

       其中: `Xcode_Coding_Signer`为证书名称 , `Xcode_Coding.app`为复制版本的Xcode名称,如起名不相同,请自行替换

    5. 到GitHub下载[Alcatraz](https://github.com/alcatraz/Alcatraz),运行, 重启Xcode_Coding , 并在第一次提示选择 `Load Bundle`

    6. 有些插件的作者没更新Xcode8的id.当用Alcatraz安装某插件,重启Xcode后没显示`Load Bundle`时,Terminal运行下面的代码,添加Xcode8的id给插件,可以解决大部分问题.

       ```
       find ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins -name Info.plist -maxdepth 3 | xargs -I{} defaults write {} DVTPlugInCompatibilityUUIDs -array-add `defaults read /Applications/Xcode_Coding.app/Contents/Info.plist DVTPlugInCompatibilityUUID`
       ```

       其中`Xcode` 可以替换成自己的 复制版本`Xcode_Coding`的名称

* #### 插件推荐

  * ##### [AMMethod2Implememt](https://github.com/MellongLau/AMMethod2Implement) :根据.h中方法的声明,在.m中自动补全方法实现.

  * ##### [Auto Importer for Xcode](https://github.com/citrusbyte/Auto-Importer-for-Xcode) :快速导入头文件.

  * #####[FastCoding-Xcode-Plugin ](https://github.com/DevDu/FastCoding-Xcode-Plugin):快速生成Getter,Setter,Lazy方法.

  * #####[FastStub-Xcode](https://github.com/music4kid/FastStub-Xcode) :快速生成.h文件中未实现的方法.

  * #####[String Editing Plugin for Xcode](https://github.com/duzexu/HOStringSense-for-Xcode) :编辑字符串.

  * #####[Xcode-Quick-Localization](https://github.com/tappollo/Xcode-Quick-Localization) :快速添加本地化与国际化文件.

  * #####[GitDiff9 - GitDiff for Xcode 9](https://github.com/johnno1962/GitDiff) :显示代码的修改变化.

  * #####[Xcode-CComment](https://github.com/flexih/Xcode-CComment) :使用C的格式注释代码.

  * #####[RegX](https://github.com/kzaher/RegX): 对齐代码格式.

  * #####[XAlign](https://github.com/qfish/XAlign#install):对齐代码格式.

  * ##### [ClangFormat-Xcode](https://github.com/travisjeffery/ClangFormat-Xcode)修改代码格式.

  * #####[SCXcodeSwitchExpander](https://github.com/stefanceriu/SCXcodeSwitchExpander): 自动补齐switch..case.

  * #####[Fui](https://github.com/dblock/fui): 自动删除无用头文件.

  * #####[CleanHeaders](https://github.com/insanoid/CleanHeaders-Xcode):自动删除无用头文件.

  * #####[Injection Plugin for Xcode](https://github.com/johnno1962/injectionforxcode):模拟器实时显示UI.

  * #####[SCXcodeMinimap v2.3](https://github.com/stefanceriu/SCXcodeMiniMap):显示代码地图.

  * #####[@AutoCompletion](https://github.com/wzqcongcong/AtAutoCompletion):自动补全数据类型.

  * #####[ATProperty](https://github.com/Draveness/ATProperty):自动补全属性类型.

  * #####[AutoHighlightSymbol](https://github.com/chiahsien/AutoHighlightSymbol):选中一个对象后,所有对象高亮.

  * #####[ESJsonFormat-Xcode](https://github.com/EnjoySR/ESJsonFormat-Xcode):将Json格式输出为模型.

  * #####[KSImageNamed-Xcode](https://github.com/ksuther/KSImageNamed-Xcode):使用图片资源显示缩略图.

  * #####[ColorSense for Xcode](https://github.com/omz/ColorSense-for-Xcode):使用颜色资源时显示颜色.

  * #####[CodePilot](https://github.com/macoscope/CodePilot):快速搜索文件.

  * #####[DXXcodeConsoleUnicodePlugin](https://github.com/dhcdht/DXXcodeConsoleUnicodePlugin):将控制台的输出编码成中文.

    ​
