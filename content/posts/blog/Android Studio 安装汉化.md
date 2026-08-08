---
title: Android Studio 安装汉化（一目了然版）
date: 2025-08-29
tags:
  - 笔记软件
url:
  - https://blog.csdn.net/weixin_58024114/article/details/150960635?spm=1001.2014.3001.5502
---
## Android Studio安装汉化        
      Android Studio 安装汉化                  
Android Studio的Marketplace并没有类IDEA的中文语言包，需要去jetbrains的插件官网里面下载对应的插件才可以使用（以2025.1.2版本为例）

### 法一：官方插件下载

插件地址：[Chinese (Simplified) Language Pack / 中文语言包 Plugin for JetBrains IDEs | JetBrains Marketplace](https://plugins.jetbrains.com/plugin/13710-chinese-simplified-language-pack----/versions) 下载好对应的包之后就可以去Android Studio导入使用了                                  
插件地址：    中文（简体）语言包 / Chinese (Simplified) Language Pack Plugin for JetBrains IDEs | JetBrains Marketplace                  下载好对应的包之后就可以去 Android Studio 导入使用了            
插件地址：    中文（简体）语言包插件 / JetBrains IDEs 中文语言包 Plugin for JetBrains IDEs | JetBrains Marketplace                  下载好对应的包之后就可以去 Android Studio 导入使用了                                    

![image-20250814102115437|494](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/image-20250814102115437.png)  

打开Android Studio，项目里依次点Settings-Plugins,然后在页面中点击上面的设置选择Install Plugin from Disk，项目外直接点击Plugins也可以进去，将刚下载好的包导入即可，然后重启Android Studio                
打开 Android Studio，项目里依次点 Settings-Plugins,然后在页面中点击上面的设置选择 Install Plugin from Disk，项目外直接点击 Plugins 也可以进去，将刚下载好的包导入即可，然后重启 Android Studio              
打开 Android Studio，在项目中依次点击 Settings-Plugins，然后在页面中点击上方的设置选择 Install Plugin from Disk，项目外直接点击 Plugins 也可以进入，将刚下载好的包导入即可，然后重启 Android Studio      
打开 Android Studio，项目里依次点 Settings-Plugins,然后在页面中点击上面的设置选择 Install Plugin from Disk，项目外直接点击 Plugins 也可以进去，将刚下载好的包导入即可，然后重启 Android Studio    
打开 Android Studio，在项目中依次点击 Settings-Plugins，然后在页面中点击上方的设置选择 Install Plugin from Disk，项目外直接点击 Plugins 也可以进入，将刚下载好的包导入即可，然后重启 Android Studio  

![image-20250814095804700](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/image-20250814095804700.png)![image-20250814095848144](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/image-20250814095848144.png)  

![image-20250814100201475](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/image-20250814100201475.png)

### 法二：使用github                            

如果官方那边没能及时跟新，这个相对来说是最好用的，因为这个是根据根据IDEA插件修改而来，兼容AndroidStudio版本，直接下载最新的即可。

地址：[[Releases · sollyu/AndroidStudioChineseLanguagePack](https://github.com/sollyu/AndroidStudioChineseLanguagePack/releases)](https://github.com/sollyu/AndroidStudioChineseLanguagePack)    

![image-20250814102237134](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/image-20250814102237134.png)  

步骤跟上述法一一样，只不过重启之后还需要设置一下语言，步骤如下：

```
项目外：自定义(Customize)->语言和地区(Language and Region)->语言(Language)->Chinese->重启AndroidStudio    
项目外：自定义(Customize)->语言和地区(Language and Region)->语言(Language)->中文->重启 Android Studio      
项目里：Settings(设置)->Appearance & Behavior(外观与行为)->System Settings(系统设置)->Language and Region(语言和地区)->语言(Language)->Chinese->重启AndroidStudio    
项目里：设置->外观与行为->系统设置->语言和地区->语言->中文->重启 Android Studio
```

项目外

![image-20250814101442870](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/image-20250814101442870.png)

项目里

![image-20250814101335584](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/image-20250814101335584.png)  

重启后即可看到已经成功汉化了
![image-20250814101614113](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/image-20250814101614113.png)
