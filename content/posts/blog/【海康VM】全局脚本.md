---
title: 【海康VM】全局脚本
date: 2025-08-06
tags:
  - 笔记软件
  - 海康
url:
  - https://blog.csdn.net/weixin_58024114/article/details/149913056?spm=1001.2014.3001.5502
---
全局脚本可用于控制多流程的运行时序、动态配置模块参数、通信触发等。支持C#语言编写，内部调用的是算法平台二次开发SDK的C#接口，可以对多流程的运行进行逻辑控制，支持接收通信数据，支持获取或修改模块参数，支持获取流程或模块运行状态和结果等。

## 界面介绍

![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/5196238ebae7483b8372e701ee4e5b29.png)
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/66d030baf1e14322a767e11ca3df70f5.png)




|                             示例                             |                           功能说明                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/ab67266603944a89ac7fb405197570c6.png)
 |                    导入之前保存的.cs文件                     |
| ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/8b51dc7dac7b4b53a183bb3ba8733350.png)
 |                     导出新生成的.cs文件                      |
| ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/4e89d3b6b1784535893104ed03a24000.png)
 | 可打开全局脚本的示例程序，分别有默认代码（即打开全局脚本时默认加载的脚本代码）、单流程执行、多流程控制执行、模块参数配置、模块运行结果获取、默全局变量设置、全局通信 |
| ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/978d1b646631407c88400fb8c74009e3.png)
 |         工程目录按钮。可使用VisualStudio调试全局脚本         |
| ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/a86736bb019946ba80cfa29c4302acba.png)
 |                       可动态添加程序集                       |
| ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/05adf2a66d26412da9d8f3f4fe6da1bb.png)
 |                         保存脚本程序                         |
| ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/d91f77f13dc54e19a0529b64182ac1c4.png)
 |            启用加密，可以设置密码，对脚本进行加密            |

预编译：编译程序，执行Init()函数； 

执行：运行Process()函数；                 

确定：保存修改后的代码并退出脚本编辑界面。

## 默认界面

```c#
using System;                   
using VM.GlobalScript.Methods;                    
using System.Windows.Forms;                       
using iMVS_6000PlatformSDKCS;                          
using System.Runtime.InteropServices;                          

public class UserGlobalScript : UserGlobalMethods,IScriptMethods                            
{                            

        public int Init()                            
        {                              
            return InitSDK();                              
        }
    
        public int Process()                            
        {                            
            if (m_operateHandle == IntPtr.Zero)                          
            {return ImvsSdkPFDefine.IMVS_EC_NULL_PTR;}                          

			int nRet = DefaultExecuteProcess();                        
			
			return nRet;                    
        }
        
        public override void ResultDataCallBack(IntPtr outputPlatformInfo, IntPtr puser)                  
        {                  
            base.ResultDataCallBack(outputPlatformInfo, puser);                
            ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO struInfo = (ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO)Marshal.PtrToStructure(outputPlatformInfo, typeof(ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO));              
            switch (struInfo.nInfoType)          
            {          

                case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_MODULE_RESULT:          
                    {          
                    	ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P resultInfo = (ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P));          
                    	break;        
                    }

                case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_WORK_STATE:      
                    {      
                        ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS stWorkStatus = (ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS));      
                        break;      
                    }
                default:      
                    break;      
            }
        }
}
```

## 执行顺序

|                    方法                    |                                功能及执行顺序描述                                |
| :--------------------------------------: | :---------------------------------------------------------------------: |
|           public int Init(){}            |                  初始化函数，会在加载方案或者编译时执行。可在此方法中 实现初始化相关操作。                  |
|          public int Process(){}          | 运行函数为主界面上运行控制按钮执行的函数，单次执行则 执行一次Process()函数，连续运行则以一定时间间隔重复执 行Process()函数 |
|    public override void  Dispose(){}     |                        资源释放。可以在关闭程序或者重新编译的时释放资源。                        |
| public override int InitAfterLoadSol(){} |                        方案加载完成后初始化函数，在方案加载完成后执行。                         |
|                                          |                                                                         |

注意：

- 以下情况不走全局脚本的Process()：

1. 硬触发不走全局脚本
2. 通信触发方案不走全局脚本，全局触发不走全局脚本
3. 控制单流程运行不走全局脚本
4. 运行界面运行控制特定流程运行时不走全局脚本（所有流程时走全局脚本）

-  Process()的运行逻辑：  

   1.全局脚本运行控制按钮单次执行时，默认代码中会调用DefaultExecuteProcess()控制所有流程 运行一次； 

   2.全局脚本运行控制按钮连续执行时，会根据设置的时间间隔循环运行process()。但是 DefaultExecuteProcess()只会在第一次时控制所有流程连续运行。所以运行控制按钮连续运行后， 运行间隔不是Process()的运行间隔决定的，是流程自身的运行间隔决定，如图 ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/f810988c946a41889cc2fcbe0c564815.png)


​    3.也可以在Process()中调用4.X的二次开发C#接口控制流程运行。using iMVS_6000PlatformSDKCS

## 接口

- 全局变量的设置和获取

|                                    接口                                     |      功能说明      |
| :-----------------------------------------------------------------------: | :------------: |
|    Int GetGlobalVariableIntValue(string paramName, ref int paramValue)    |   获取int型全局变值   |
|     Int SetGlobalVariableIntValue (string paramName, int paramValue)      |   设置int型全局变值   |
|  Int GetGlobalVariableFloatValue(string paramName, ref float paramValue)  | 获取float型全局变量值  |
|   Int SetGlobalVariableFloatValue (string paramName, float paramValue)    | 设置float型全局变量值  |
| nt GetGlobalVariableStringValue (string paramName, ref string paramValue) | 获取string型全局变量值 |
|  Int SetGlobalVariableStrignValue (string paramName, string paramValue)   | 设置string型全局变量值 |

- 全局脚本运行时间

|                            接口                            |           功能说明           |
| :--------------------------------------------------------: | :--------------------------: |
|         uint GetScriptContinuousExecuteInterval();         | 获取全局脚本连续运行时间间隔 |
| void SetScriptContinuousExecuteInterval(uintnMilliSecond); | 设置全局脚本连续运行时间间隔 |

- 通信相关接口

  通信接收

|                             接口                             |     功能说明     |
| :----------------------------------------------------------: | :--------------: |
|                bool StartGlobalCommunicate()                 |  初始化全局通信  |
|         Void RegesiterReceiveCommunicateDataEvent()          | 注册通信接收事件 |
|        Void UnRegesiterReceiveCommunicateDataEvent()         | 注销通信接收事件 |
| Void  UserGlobalMethods_OnReceiveCommunicateDataEvent(ReceiveDat aInfo dataInfo) | 通信数据接收事件 |

​	发送数据

|                             接口                             |                 功能说明                 |
| :----------------------------------------------------------: | :--------------------------------------: |
|         SendCommDeviceData(string data,intdeviceID)          | TCP、串口、udp发送int、float、string数据 |
|       SendCommDeviceData(byte[] bytedata,int deviceID)       |      Tcp、串口、udp发送十六进制数据      |
| SendCommDeviceData(string data,int deviceID,int addressID,DataType dataType) |  PLC、Modbus发送int、float、string数据   |
| SendCommDeviceData(byte[] bytedata,int deviceID,int addressID,DataType.ByteType) |       PLC、Modbus发送十六进制数据        |

## 调试

全局脚本的调试步骤和脚本的调试步骤大差不差，也是需要导出然后使用Visual Studio进行调试，具体如下：

1. 点击工程目录按钮，选择.sln文件双击（右键）使用Visual Studio打开，然后设置断点并生成解决方案

   ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/ec4c3d7a550e4bd08ac766e488c5b6de.png)


   

2. 调试选项中左键点击附加到进程，在可用进程选项中寻找GlobalScript.exe点击右下角附加按钮

   ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/efdce877b6e54cdf97a00b814957d1aa.png)


3. 点击全局执行按钮，同时查看VS中是否运行进入断点，若进入断点则表示全局脚本可以使用VS正常调试

   ![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/80a873e04dd142e4889135db26035ad7.png)


​    注意：在VS中对脚本的代码进行修改和调试

​	（1）点击停止调试，在VS中修改代码后，重新生成即可将修改后的代码同步到VM中。 

​	（2）需要继续进行调试时，需要在VS中重新选择附加到进程。不能直接点击启动。

## 案列示例

### 案例一：单流程执行

```c#
using System; //该命名空间包含基础类  
using VM.GlobalScript.Methods;//包含全局脚本中使用的自定义方法或工具类  
using System.Windows.Forms;//该命名空间用于创建Windows桌面应用程序，提供了各种窗口、控件和事件处理机制
using iMVS_6000PlatformSDKCS;//用于与iMVS 6000平台进行交互的SDK，提供了访问平台功能的接口和类  
using System.Runtime.InteropServices;//该命名空间提供了一组服务，使托管代码可以与非托管代码进行交互，通常用于调用DLL中的函数。


//示例说明: 运行流程1
 
public class UserGlobalScript : UserGlobalMethods,IScriptMethods  
{  
        public int Init()    
        {  
        	//二次开发SDK初始化
            return InitSDK();  
        }

        /// 单次执行:该函数执行一次
        public int Process()  
        {  
        	//m_operateHandle 二次开发SDK操作句柄  
            if (m_operateHandle == IntPtr.Zero)  
            {return ImvsSdkPFDefine.IMVS_EC_NULL_PTR;}  
		    
			//自定义执行逻辑
            //流程1运行一次
			int nRet = ImvsPlatformSDK_API.IMVS_PF_ExecuteOnce_V30_CS(m_operateHandle,10000,null);  
			return nRet;  
        }
        
        /// SDK回调函数
        public override void ResultDataCallBack(IntPtr outputPlatformInfo, IntPtr puser)
        {
			base.ResultDataCallBack(outputPlatformInfo, puser);
            ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO struInfo = (ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO)Marshal.PtrToStructure(outputPlatformInfo, typeof(ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO));
            switch (struInfo.nInfoType)
            {
                //获取模块结果数据
                case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_MODULE_RESULT:
				    {
                    	ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P resultInfo = (ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P));
                   	    break;
				    }
                ///获取流程运行状态
                case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_WORK_STATE:
                    {
                        ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS stWorkStatus = (ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS));
                        break;
                    }
                default:
                    break;
            }
        }
}
```

![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/5c07bb2ed41d440baf2b13643697f7e3.png)


点击执行后，可以看到流程1执行了一次，但是流程2并没有执行

### 案例二：多流程执行

```c#
using System;
using VM.GlobalScript.Methods;
using System.Linq;
using System.Windows.Forms;
using iMVS_6000PlatformSDKCS;
using System.Runtime.InteropServices;
using System.Threading;
using System.Collections.Generic;


//示例说明:执行流程1，流程2,都执行完后执行流程3，流程3执行完后再执行流程1一次

public class UserGlobalScript : UserGlobalMethods, IScriptMethods
{
    public int Init()
    {
        //二次开发SDK初始化
        InitSDK();
        //设置全局脚本连续运行时间间隔为200ms
        SetScriptContinuousExecuteInterval(200);
		
		return 0;
    }

    /// 单次执行:该函数执行一次
    public int Process()
    {
    	//m_operateHandle 二次开发SDK操作句柄
        if (m_operateHandle == IntPtr.Zero)
        { return ImvsSdkPFDefine.IMVS_EC_NULL_PTR; }

        //自定义执行逻辑 
        //1.流程1,流程2 同步执行一次
        ExecuteMultiProcessOnceSync(new uint[] { 10000, 10001 });
        //2.同步执行流程3一次
        ExecuteSingleProcessOnceSync(10002);
        //3.同步执行流程1一次
        ExecuteSingleProcessOnceSync(10000);
		
		return 0;
    }


    /// 添加流程ID至需要同步的键值对
    /// <param name="processIDArray"></param>
    public void AddProcessIDToDictResetEvent(uint[] processIDArray)
    {
        if (processIDArray == null || processIDArray.Length == 0)
        {
            return;
        }
        for (int i = 0; i < processIDArray.Length; i++)
        {
            if (!dictProcessExecuteResetEvent.ContainsKey(processIDArray[i]))  
            {
                dictProcessExecuteResetEvent.Add(processIDArray[i], new AutoResetEvent(false));
            }
        }
    }

    /// 单流程同步运行一次
    /// <param name="processID">进程ID</param>  
    public bool ExecuteSingleProcessOnceSync(uint processID)
    {
        bool bret = false;
        //如果未包含，需要添加进去
        if (!dictProcessExecuteResetEvent.ContainsKey(processID))
        {
            dictProcessExecuteResetEvent.Add(processID, new AutoResetEvent(false));
        }
        //流程执行一次
        int ret = ImvsPlatformSDK_API.IMVS_PF_ExecuteOnce_V30_CS(m_operateHandle, processID, null);
        if (ret == 0)
        {
            //阻塞在这里,等待流程运行结束
            dictProcessExecuteResetEvent[processID].WaitOne();
            bret = true;    
        }
        else          
        {        
            bret = false;
        }
        return bret;
    }

    /// 多个流程同步运行一次
    /// <param name="processIDArray">流程ID集合</param>
    public void ExecuteMultiProcessOnceSync(uint[] processIDArray)
    {
        if (processIDArray == null || processIDArray.Length == 0)
        {
            return;
        }
        for (int i = 0; i < processIDArray.Length; i++)
        {
            //如果未包含该键值需要添加
            if (!dictProcessExecuteResetEvent.ContainsKey(processIDArray[i]))
            {
                dictProcessExecuteResetEvent.Add(processIDArray[i], new AutoResetEvent(false));
            }
            if (0 != ImvsPlatformSDK_API.IMVS_PF_ExecuteOnce_V30_CS(m_operateHandle, processIDArray[i], null))
            {
                //如果执行失败，则不进行触发
                dictProcessExecuteResetEvent[processIDArray[i]].Set();
            }
        }
        WaitHandle[] arrayHandle = dictProcessExecuteResetEvent.Where(x => (processIDArray.ToList().Contains(x.Key))).Select(x => x.Value).ToArray();
        //等待多个流程运行结束
        AutoResetEvent.WaitAll(arrayHandle);
    }

    ///流程运行状态空闲回调，如果收到空闲回调的信息则进行autoRestEvent的触发
    public void ExecuteProcessSyncCallBack(ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS workStatus)
    {
        //1为忙碌状态，0位空闲状态，为0时说明流程执行完毕
        if (workStatus.nWorkStatus != 0)
            return;
        if (dictProcessExecuteResetEvent != null && dictProcessExecuteResetEvent.Count != 0)
        {
            if (dictProcessExecuteResetEvent.ContainsKey(workStatus.nProcessID))  
            {      
                //流程运行结束，触发主线程运行
                dictProcessExecuteResetEvent[workStatus.nProcessID].Set();                
            }
        }
    }

    /// SDK回调函数
    public override void ResultDataCallBack(IntPtr outputPlatformInfo, IntPtr puser)            
    {          
		base.ResultDataCallBack(outputPlatformInfo, puser);          
        ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO struInfo = (ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO)Marshal.PtrToStructure(outputPlatformInfo, typeof(ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO));          
        switch (struInfo.nInfoType)          
        {          
            //获取模块结果数据
            case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_MODULE_RESULT:          
                {          
                    ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P resultInfo = (ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P));          
                    break;          
                }
            ///获取流程运行状态
            case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_WORK_STATE:          
                {          
                    ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS stWorkStatus = (ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS));          
                    //处理流程运行状态
                    ExecuteProcessSyncCallBack(stWorkStatus);          
                    break;        
                }
            default:        
                break;        
        }
    }
}
```

![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/bf770831848749789f3b12f9581a3f25.png)


点击运行后，可以看到流程1，2，3都执行了

### 案例三：全局通信

```c#
using System; 
using VM.GlobalScript.Methods;
using System.Windows.Forms; 
using iMVS_6000PlatformSDKCS;
using System.Runtime.InteropServices;

//示例说明: 接收来自全局通信模块接收到的数据,如果接收到数据字符0，则执行流程1一次

public class UserGlobalScript : UserGlobalMethods,IScriptMethods
{
        public int Init()
        {
        	//二次开发SDK初始化
            InitSDK();
            //设置与全局通信模块的通信端口
            StartGlobalCommunicate();
            //注册通信数据接收事件
            RegesiterReceiveCommunicateDataEvent();
			
			return 0;
        }

        /// 单次执行:该函数执行一次
        public int Process()
        {
        	//m_operateHandle 二次开发SDK操作句柄
            if (m_operateHandle == IntPtr.Zero)
            {return ImvsSdkPFDefine.IMVS_EC_NULL_PTR;}
		
		    //默认执行全部流程，如果自定义流程执行逻辑，请移除DefaultExecuteProcess方法
			int nRet = DefaultExecuteProcess();
			
			return nRet;
        }

        public override void UserGlobalMethods_OnReceiveCommunicateDataEvent(ReceiveDataInfo dataInfo)
        {
        	if(dataInfo == null || dataInfo.DeviceData==null)
        	{return;}
        	//接收到的数据转成字符串
        	string str = System.Text.Encoding.Default.GetString(dataInfo.DeviceData);
        	//这里的deviceIndex和全局通信模块中的一致（即设备列表中的序号）
        	if(dataInfo.DeviceID==1)
            {
        		//解析收到的数据
        		if(str=="0")
        	    {
        			//执行流程1 一次
        			ImvsPlatformSDK_API.IMVS_PF_ExecuteOnce_V30_CS(m_operateHandle,10000,null);//10000表示流程1
        		}
              
        	}
        }
        
        /// SDK回调函数
        public override void ResultDataCallBack(IntPtr outputPlatformInfo, IntPtr puser)
        {
            base.ResultDataCallBack(outputPlatformInfo, puser);
            ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO struInfo = (ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO)Marshal.PtrToStructure(outputPlatformInfo, typeof(ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO));
            switch (struInfo.nInfoType)
            {  
                //获取模块结果数据
                case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_MODULE_RESULT:
                	{
                    	ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P resultInfo = (ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P));  
                    	break;  
                    }
                ///获取流程运行状态
                case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_WORK_STATE:              
                    {  
                        ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS stWorkStatus = (ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS));  
                        break;  
                    }
                default:  
                    break;  
            }
        }
}
```

首先需要先建立通信连接，我这里使用的工具是HslCommunicationDemo，这是一个可以二次开发的通讯库，您可以基于这个通信库非常简单，便捷的和工业现场的设备进行读写操作，而不用关心底层的细节，可以帮助您快速，高质量的交付工业软件（PS:[胡工科技官网](http://www.hsltechnology.cn/)有需要的可以去下载使用），建立成功后点击发送字符0，可以看到流程1执行了一次（当然也可以控制多流程同时执行，执行指定流程，设置全局变量等操作）

![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/df1c95aebeb44ce8b6801b17fccfe96e.png)  


![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/17f7c69d05d740f4842485c5ad75fd86.png)


### 案例四：设置全局变量值

```c#    
using System;       
using VM.GlobalScript.Methods;      
using System.Windows.Forms;       
using iMVS_6000PlatformSDKCS;    
using System.Runtime.InteropServices;  

//示例说明:获取全局变量的三种类型值，然后设置全局变量的三种类型值

public class UserGlobalScript : UserGlobalMethods,IScriptMethods  
{  
        public int Init()  
        {  
        	//二次开发SDK初始化
            return InitSDK();  
        }
        /// 单次执行:该函数执行一次
        public int Process()  
        {  
        	//m_operateHandle 二次开发SDK操作句柄  
            if (m_operateHandle == IntPtr.Zero)  
            {return ImvsSdkPFDefine.IMVS_EC_NULL_PTR;}  
		
            //全局变量参数获取
            int iGlobalVar =-1;  
            float fGlobalVar=0f;  
            string strGlobalVar="";  
            GetGlobalVariableIntValue("var0",ref iGlobalVar);  
            GetGlobalVariableFloatValue("var1",ref fGlobalVar);
            GetGlobalVariableStringValue("var2",ref strGlobalVar);
            //MessageBox.Show(String.Format("Get GlobalVariable var0: {0} var1: {1} var2: {2}",
                                           //iGlobalVar.ToString(),fGlobalVar.ToString(),strGlobalVar));
            //全局变量参数设置            
            SetGlobalVariableIntValue("var0",100);
            SetGlobalVariableFloatValue("var1",3.14f);
            SetGlobalVariableStringValue("var2","hello");
			
			return 0;
        }
        
        /// SDK回调函数
        public override void ResultDataCallBack(IntPtr outputPlatformInfo, IntPtr puser)
        {
            base.ResultDataCallBack(outputPlatformInfo, puser);
            ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO struInfo = (ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO)Marshal.PtrToStructure(outputPlatformInfo, typeof(ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO));
            switch (struInfo.nInfoType)
            {
                //获取模块结果数据
                case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_MODULE_RESULT:
                    {
                        ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P resultInfo = (ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P));
                        break;
                    }
                ///获取流程运行状态
                case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_WORK_STATE:
                    {
                        ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS stWorkStatus = (ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS));
                        break;
                    }
                default:
                    break;
            }
        }
}
```

![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/4ce2b7768b4b4913a7a012ef3468b614.png)


点击执行后，可以看到全局变量的值已经同步设置了

![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/c41fd57fe42e4d4b941f680a3d02a260.png)


### 案例五：模块运行结果获取

```c#
using System;
using VM.GlobalScript.Methods;
using System.Linq;
using System.Windows.Forms;
using iMVS_6000PlatformSDKCS;
using System.Runtime.InteropServices;
using System.Threading;//多线程
using System.Collections.Generic;//泛型集合

 //说明：执行流程1，如果流程1中的条件检测模块结果为OK，则执行流程2，否则不执行
public class UserGlobalScript : UserGlobalMethods, IScriptMethods
{
	//记录条件检测模块的输出结果
	private string iIFModuleOutResult = "";
    private bool bGlobalRun = false;    
	
    public int Init()
    {
        //二次开发SDK初始化
        InitSDK();
        //设置全局脚本连续运行时间间隔为200ms
        SetScriptContinuousExecuteInterval(200);
		
		return 0;
    }

    public int Process()
    {
    	//m_operateHandle 二次开发SDK操作句柄
        if (m_operateHandle == IntPtr.Zero)
        { return ImvsSdkPFDefine.IMVS_EC_NULL_PTR;}
        bGlobalRun = true;
        //默认开启条件检测结果回调
        ImvsPlatformSDK_API.IMVS_PF_CtrlCallBackModuResult_CS(m_operateHandle,2,1);
        //自定义执行逻辑 
        //1.同步运行流程1
        ExecuteSingleProcessOnceSync(10000);
		//如果条件检测模块执行结果为OK
        if(iIFModuleOutResult =="OK")
        {
        	//2.同步执行流程2
            ExecuteSingleProcessOnceSync(10001);
        }
        //运行完后重置结果
        iIFModuleOutResult = "";
		bGlobalRun = false;
		return 0;
    }

    /// 添加流程ID至需要同步的键值对
    /// <param name="processIDArray"></param>
    public void AddProcessIDToDictResetEvent(uint[] processIDArray)
    {
        if (processIDArray == null || processIDArray.Length == 0)
        {
            return;
        }
        for (int i = 0; i < processIDArray.Length; i++)
        {
            if (!dictProcessExecuteResetEvent.ContainsKey(processIDArray[i]))
            {
                dictProcessExecuteResetEvent.Add(processIDArray[i], new AutoResetEvent(false));
            }
        }
    }


    /// 单流程同步运行一次
    /// <param name="processID">进程ID</param>
    public bool ExecuteSingleProcessOnceSync(uint processID)
    {
        bool bret = false;
        //如果未包含，需要添加进去
        if (!dictProcessExecuteResetEvent.ContainsKey(processID))
        {
            dictProcessExecuteResetEvent.Add(processID, new AutoResetEvent(false));
        }
        //流程执行一次
        int ret = ImvsPlatformSDK_API.IMVS_PF_ExecuteOnce_V30_CS(m_operateHandle, processID, null);
        if (ret == 0)
        {
            //阻塞在这里,等待流程运行结束
            dictProcessExecuteResetEvent[processID].WaitOne();
            bret = true;
        }
        else
        {
            bret = false;
        }
        return bret;
    }
    
    /// 多个流程同步运行一次
    /// <param name="processIDArray">流程ID集合</param>
    public void ExecuteMultiProcessOnceSync(uint[] processIDArray)
    {
        if (processIDArray == null || processIDArray.Length == 0)
        {
            return;
        }
        for (int i = 0; i < processIDArray.Length; i++)
        {
            //如果未包含该键值需要添加
            if (!dictProcessExecuteResetEvent.ContainsKey(processIDArray[i]))
            {
                dictProcessExecuteResetEvent.Add(processIDArray[i], new AutoResetEvent(false));
            }
            if (0 != ImvsPlatformSDK_API.IMVS_PF_ExecuteOnce_V30_CS(m_operateHandle, processIDArray[i], null))
            {
            //如果执行失败，则不进行触发
                dictProcessExecuteResetEvent[processIDArray[i]].Set();
            }
        }
        WaitHandle[] arrayHandle = dictProcessExecuteResetEvent.Where(x => (processIDArray.ToList().Contains(x.Key))).Select(x => x.Value).ToArray();
        //等待多个流程运行结束
        AutoResetEvent.WaitAll(arrayHandle);
    }


    ///流程运行状态空闲回调，如果收到空闲回调的信息则进行autoRestEvent的触发
    public void ExecuteProcessSyncCallBack(ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS workStatus)
    {
        //1为忙碌状态，0位空闲状态，为0时说明线程执行完毕
        if (workStatus.nWorkStatus != 0)
            return;
        if (dictProcessExecuteResetEvent != null && dictProcessExecuteResetEvent.Count != 0)
        {
            if (dictProcessExecuteResetEvent.ContainsKey(workStatus.nProcessID))
            {
                //流程运行结束，触发主线程运行
                dictProcessExecuteResetEvent[workStatus.nProcessID].Set();
            }
        }
    }


    /// SDK回调函数
    public override void ResultDataCallBack(IntPtr outputPlatformInfo, IntPtr puser)
    {
		base.ResultDataCallBack(outputPlatformInfo, puser);
        ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO struInfo = (ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO)Marshal.PtrToStructure(outputPlatformInfo, typeof(ImvsSdkPFDefine.IMVS_PF_OUTPUT_PLATFORM_INFO));
        switch (struInfo.nInfoType)
        {
            //获取模块结果数据
            case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_MODULE_RESULT:
                {
                    ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P resultInfo = (ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P));
                    //流程ID判断                    
                    if(resultInfo.nProcessID == 10000)
                    {
                    	//对模块结果数据进行处理
                    	UpdateDataModuResutOutput(resultInfo);
                    }
                    break;
                }
            ///获取流程运行状态
            case (uint)ImvsSdkPFDefine.IMVS_CTRLC_OUTPUT_PlATFORM_INFO_TYPE.IMVS_ENUM_CTRLC_OUTPUT_PLATFORM_INFO_WORK_STATE:
                {
                    ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS stWorkStatus = (ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS)Marshal.PtrToStructure(struInfo.pData, typeof(ImvsSdkPFDefine.IMVS_PF_MODULE_WORK_STAUS));
                    //处理流程运行状态
                    if(bGlobalRun)
                    {ExecuteProcessSyncCallBack(stWorkStatus);}
                    break;
                }
            default:
                break;
        }
    }
    

    /// 模块结果数据处理函数
    private void UpdateDataModuResutOutput(ImvsSdkPFDefine.IMVS_PF_MODULE_RESULT_INFO_LIST_P struResultInfo)
    {
    	try
    	{
    		 //获取模块3条件检测的状态
	    	if(struResultInfo.nModuleID ==2 )
	    	{
	    		for (int i = 0; i < struResultInfo.nResultNum; i++)
	    		{
	    			if(struResultInfo.pstModuResInfo[i].strParamName=="StrResult")
	    			{
	    				if(struResultInfo.pstModuResInfo[i].nValueNum>0)
	    				{
	    					//获取模块结果
	    		        	iIFModuleOutResult = struResultInfo.pstModuResInfo[i].pstStringValue[0].strValue;
	    				}
	    			}
	    		}
	    	}
    	}
    	catch  
        {}
    }
}
```

## 注意事项

1. 脚本中不支持调用控制器管理里面的设备发送IO数据； 
2. 脚本模块尽量减少关于系统资源的操作，因为可能会导致重编译时候资源泄漏导致的异常，包括线程、串口端口、文件 等；
3. 尽量只做后台业务，不涉及界面层；
4. 尽量不在脚本中操作非托管资源，如果要操作非托管资源，则需要在process定义并且释放；

