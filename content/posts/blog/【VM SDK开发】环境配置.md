---
title: 【VM SDK开发】环境配置
date: 2025-07-29
tags:
  - 笔记软件
  - 海康
url:
  - https://blog.csdn.net/weixin_58024114/article/details/149723278?spm=1001.2014.3001.5502
---
# 前言
VisionMaster算法平台SDK V4.2版本基于方案、流程和模块工具等进行对象级封装，可通过方案、流程和模块工具中各个对象的功能接口进行相应的数据交互与运行控制。同时提供相应的流程配置控件、参数配置控件、渲染控件、全局工具控件以及前端运行界面控件等，方便进行方案编辑、参数配置以及渲染展示，便于灵活进行开发，并扩展机器视觉应用。
# visionmaster的下载与VisualStudio下载  

## visionmaster下载    

 1. 首先进入[海康机器人官网](https://www.hikrobotics.com/cn)，点击机器视觉然后点击视觉软件中的算法平台，接着点击软件下载进入下载界面，然后就可以选择所需版本了，本文演示版本为4.2
![安装1](https://i-blog.csdnimg.cn/direct/c2d748048934453999d9c51d47a45cc5.png)
![安装2](https://i-blog.csdnimg.cn/direct/3a1ab32c2caf41f78be44fb29ac4350e.png)
![安装3](https://i-blog.csdnimg.cn/direct/1fc80889871d47878d966b63cef9f2d8.png)
 2. 各个安装包的说明
 		基础安装包：基本的功能都有，比如采集，定位，测量，识别等
 		深度学习安装包：进行深度学习训练时需要安装
 		示例补丁包：里面是一些示例案例方案，安装后则可直接打开对应方案进行学习
 3. 下载好进入安装界面，这里有几种安装授权方式，如果有加密狗可以选择加密狗，没有可以去下教育版也是可以进去的。我这里由于有加密狗所以选择的是加密狗加密。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/176038020c034776bf65526ecd81ea3d.png)   
软加密：通过软加密使用软件时，仅在安装PC首次使用时通过加密文件进行激活即可。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0c647b742544407fa09820aab7753f30.png)
加密狗：通过加密狗使用软件时，每次使用前需确保安装软件的PC已通过USB接口插上加密狗。
 4. 安装完成进入首页如图所示
 ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4cd2292a1c9a4921a6b1689810eb0e87.png)
## visualstudio下载  
这个的下载安装比较简单，进入官网下载网址（[visualstudio下载](https://visualstudio.microsoft.com/zh-hans/downloads/)），选择一个版本即可，这里我选择是是Community版本，下载完后直接安装就可以了，我这里下载的是2022版本。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6a1679be17484dceaf42b411ac866a98.png)
安装时可以根据需求进行安装，但必须跟桌面相关的都要安装，否则则无法完成后续开发。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8972a301df1340b6ba700c20e711ef04.png)

# SDK开发环境的配置（以winform为例）
主要分为三步：新建工程，添加引用，添加控件

## 新建工程
推荐VS2017及以上（官方要求必须是2013以上）

 1. 打开visualstudio选择新建项目
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/43b5e3d10fd64024b4a09cab890ee7b1.png)
 2. 选择Windows窗体应用带NET的那个，因为VM二次开发是基于NET框架实现的，然后下一步![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/69fa8ab1153d4ad2b209af335610db4d.png)
 3. 编辑项目名称以及选择框架（要选择4.6及以上版本的），并将将解决方案和项目放在同一目录勾上，然后点击创建。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/60a3ae8ccf3c4ec1a25fdbcbb785a1f8.png)

     注意项目路径不能包含特殊字符！！！

 4. 进入界面如下所示
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9ace12cc7a3d463d93f17eaf59052b95.png)

 5. 选择调试-项目属性-生成界面-【首选32位】去掉勾选
 ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/443ea9a4bae8495fbc60b1716c2f09f3.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f36759fce7384ed0a8c2d3777f31bd97.png)
## 添加引用
运行导入工具添加引用，路径VisionMaster4.2.0\Development\V4.x\ComControls\Tool\ImportRef.exe
1. 选择刚刚的项目路径，然后按照需求勾选完后点击>>，然后点击确定就好了
 ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cb8d4594bbea42928ea5337f403e1153.png)
 2. 选择全部重新加载即可，然后引用区就会出现刚刚勾选的了
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9ce9963773b945efbc005dfa8d5202cf.png)
 ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/18645b248991459da7f10fad67fad3d2.png)
## 添加控件
1. 打开工具箱（没有的话就是没有显示，依次点击视图-工具箱就可以显示出来了），所有Windows窗体右击--选择项
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4b21bffb17a844b1a94aec035cd67dba.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0edc406c53b740acb2cdd16637f7a284.png)

2. 选择.NET组件，点击浏览选择visionmaster安装路径VisionMaster4.2.0\Development\V4.x\ComControls\Assembly，选择
VMControls.Winform.Release.dll，然后打开后点击确定即可
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/faefb91e7fcc466494079b33975b5352.png)
3. 添加成功前后对比如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8d5479c970c74d908e8201e5f6d773b7.png)
各个控件说明
> VmFrontendControl 前端运行界面控件
> VmGlobalToolControl 全局模块控件 	 	
> VmMainViewConfigControl 主界面控件 	
> VmParams 参数配置控件 	 	
> VmParamsConfigWithRender 参数配置带渲染控件  	
> VmProcedureConfigControl 流程配置控件 	 	
> VmRealTimeAcqControl实时取流控件(V4.2新增)  	 	
> VmRenderControl 渲染控件  	 	
> VmSingleModuleSetConfigControl 独立Group控件(V4.2新增)




































