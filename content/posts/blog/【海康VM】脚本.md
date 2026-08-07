---
title: 【海康VM】脚本
date: 2027-08-07
tags:
  - 海康
  - 软件使用
url:
  - https://blog.csdn.net/weixin_58024114/article/details/149811713?spm=1011.2415.3001.5331
---
# 脚本简介
在VM中，脚本作为一个模块在VM流程中存在，使用C#编程，可在VS进行代码的编写、调试。支持int、flaot、string、Bytes和Image五种数据类型的输入输出。在脚本中，可自行实现功能，也可调用第三方库。除了对数据的处理外，还可在脚本中对VM全局变量获取和设置、模块的运行参数设置，也可在脚本中调用通信设备发送数据等。

## 编辑界面
编辑界面如下：
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/c0977ad7014c46a5ae981003124a8f36.png)   

 1. 输入、输入变量编辑区域（1，2）：对输入输出的变量进行编辑，支持自定义变量名称，支持Int、Float、String、Bytes和IMAGE五种数据型。输入可绑定前项模块的结果、全局变量等。输出变量的值可供脚本后续模块使用。
 2. C #编程区域 （4）：提供默认代码，用户可在public partial class UserScript:ScriptMethods,IProcessMethods中实现自己的方法。默认代码中，Init()函数为初始化函数，Porcess()为处理函数。Init()函数只有在加载方案或者修改代码后预编译时才会运行，Porcess()在流程执行的时候运行。                                                      
3. C #编程区域 （4）：提供默认代码，用户可在 public partial class UserScript:ScriptMethods,IProcessMethods 中实现自己的方法。默认代码中，Init()函数为初始化函数，Porcess()为处理函数。Init()函数只有在加载方案或者修改代码后预编译时才会运行，Porcess()在流程执行的时候运行。                                  
 4. 功能区（3）：

> 导入：导入脚本程序.cs文件； 
> 导出：导出脚本程序.cs文件； 
>  编辑程序集：可动态添加程序集； 
> 导出工程：导出后，可使用VS调试脚本程序
 5. 提示区（5）：编译执行的结果显示
 6. 菜单区（6）：
> 预编译：对程序进行预编译，并调用Init()函数；
>  执行：运行Process()函数；                                                            
> 执行：运行 Process()函数；                                                            
>  确定：保存修改后的代码并退出脚本编辑界面
##  执行顺序
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/f7aad82dc7524cfbab7de5bb29f56eeb.png)
编辑完成后，点击预编译，此时编译脚本并运行初始化函数Init()。然后点击执行会运行Process()函数。
**脚本中方法执行顺序**：
1. 加载方案时，会先编译脚本并调用Init()函数。开启静默执
行的情况下，还会调用一次Process()函数；
2. 方案加载成功后，在不修改脚本的情况下，每次流程执行
只会运行Process()函数；                                                                                                                    
只会运行 Process()函数；                                          

综上：可在Init()实现初始化部分，相关工作会在加载方案的
时候完成（例如：变量初始化、”创建句柄”等）；Process()中
实现具体的功能，可以保证实现的功能在流程运行都会执行
（例如：变量计算、逻辑处理等）。

## 调试

 1. 使用VisualStudio调试脚本，编辑完后，点击导出工程，然后右击文件选择VisualStudio打开（双击）
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/370686b297144bcf82b9591107fcfb43.png)
 2. 设置相应断点并生成解决方案
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/068ba5276f5c4990889cadc27772280a.png)
 3. 点击调试--附加到进程--选择VmModuleProxy.exe（默认）（注意：每一次调试之前都需要重新附加到进程）
 	
> 脚本模块有两种执行模式，分别为exe模式和com模式。同时也可设置是否适用输入/输出变量。通过安装路径下的ShellConfig.ini配置文件进行修改，文件所在路径为：C:\Program\ Files\VisionMaster+版本号\Applications\Module(sp)\x64\Logic\ShellModule。                                          
脚本模块有两种执行模式，分别为 exe 模式和 com 模式。同时也可设置是否适用输入/输出变量。通过安装路径下的 ShellConfig.ini 配置文件进行修改，文件所在路径为：C:\Program Files\VisionMaster+版本号\Applications\Module(sp)\x64\Logic\ShellModule。  

com模式：软件默认使用的模式，可附加进程到 VmModuleProxy.exe。         
exe模式：在VS界面上方调试选项中左键点击附加到进程，在可用进程选项中寻找ShellModuleManager.exe附加到程序中。由于方案中可能存在多个脚本模块因此需使用任务管理器确定脚本模块的PID，找到对应的进程附加到程序中
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/42289d4854b94a03960cbfa064794bc9.png)
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/9d50f6a535814d1f8e91076ff670864e.png)
 1. VM点击单次执行，同时查看VS中是否进入断点，如图所示，成功进入会有提示，则表示脚本可以正常支持调试
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/0a15a6d9138b4c9cba91a76c6e2864dd.png)
 2. 注意事项
（1）点击停止调试，在VS中修改代码后，重新生成即可将修改后的代码同步到VM中。
（2）需要继续进行调试时，需要在VS中重新选择附加到进程。不能直接点击启动。

## 脚本接口
函数接口分为数据获取接口、数据输出接口、调试相关的接口。

### 设置/获取全局变量接口

|        |                                方法                                 |                  说明                   |
| :----- | :---------------------------------------------------------------: | :-----------------------------------: |
| 设置全局变量 | GlobalVariableModule.SetValue(string paramName,string paramValue) | paramName：全局变量名称 paramValue：全局变量值（下同） |
| 获取全局变量 |      object GlobalVariableModule.GetValue (string paramName)      |                                       |

### 获取模块结果数据: GetModule.SetValue            

|                                        方法                                         |                     说明                      |
| :-------------------------------------------------------------------------------: | :-----------------------------------------: |
| CurrentProcess.GetModule(string paramModuleName).GetValue(string  paramValueName) | paramModuleName：模块名称paramValueName：参数名称（下同） |

GetModule中传入参数为模块名称，如果模块存在Group中，需要加上Group的名称形如：CurrentProcess.GetModule("Group1.图像源1")，GetValue中传入为模块输出参数名称。	     
GetModule 中传入参数为模块名称，如果模块存在 Group 中，需要加上 Group 的名称形如：CurrentProcess.GetModule("Group1.图像源 1")，GetValue 中传入为模块输出参数名称。	       
GetModule 中传入参数为模块名称，如果模块存在于 Group 中，需要加上 Group 的名称，例如：CurrentProcess.GetModule("Group1.图像源 1")，GetValue 中传入的是模块输出参数名称。  

### 设置模块运行参数: GetModule.SetValue              

|                                                 方法                                                  |       说明       |
| :-------------------------------------------------------------------------------------------------: | :------------: |
| CurrentProcess.GetModule(string paramModuleName).SetValue(string  paramValueName，string paramValue) | paramValue：参数值 |

GetModule中传入参数为模块名称并且若存在Group则在传入名称时要加上Group形如：CurrentProcess.GetModule("Group1.图像源1")，GetValue中传入为模块参数名称以及参数值。	         
GetModule 中传入参数为模块名称，并且若存在 Group，则在传入名称时要加上 Group，形如：CurrentProcess.GetModule("Group1.图像源 1")，GetValue 中传入为模块参数名称以及参数值。
GetModule 中传入参数为模块名称，并且若存在 Group，则在传入名称时要加上 Group，形如：CurrentProcess.GetModule("Group1.图像源 1")，GetValue 中传入为模块参数名称以及参数值。             
### 通信发送数据接口

#### PLC、Modbus通信接口：SendData(byte[] bytedata,DataType.ByteType)          
      PLC、Modbus 通信接口：SendData(byte[] bytedata, DataType.ByteType)            
      PLC、Modbus 通信接口：SendData(byte[] bytedata, DataType.ByteType)            
      PLC、Modbus 通信接口：SendData(byte[] bytedata, DataType.ByteType)              

|                功能                |                                                           方法                                                            |                                                     说明                                                     |
| :------------------------------: | :---------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------: |
| PLC、Modbus发送Int、float、string类型数据 |   GlobalCommunicateModule.GetDevice(int deviceID).GetAddress(int  addressID).SendData(string data,DataType dataType)    | deviceID：通信管理中设备ID；addressID：设备地址ID；data：待发送的数据，如果发送多个，请用“；”隔开；dataType：待发送数据类型，包含int，float，string三种类型（下同） |
|        PLC、Modbus发送十六进制数据        | GlobalCommunicateModule.GetDevice(int  deviceID).GetAddress(int addressID).SendData(byte[]  bytedata,DataType.ByteType) |                                            bytedata：待发送的十六进制数据                                             |

#### TCP、UDP、串口发送String类型数据接口：SendData(string data)              
      TCP、UDP、串口发送 String 类型数据接口：SendData(string data)  
      TCP、UDP、串口发送 String 类型数据接口：SendData(string data)          
      TCP、UDP、串口发送 String 类型数据接口：SendData(string data)            

|      功能      |                                    方法                                     |        说明         |
| :----------: | :-----------------------------------------------------------------------: | :---------------: |
| 发送String类型数据 |   GlobalCommunicateModule.GetDevice(int deviceID).SendData(string data)   |                   |
|   发送十六进制数据   | GlobalCommunicateModule.GetDevice(int deviceID).SendData(byte[] bytedata) | bytedata：待发送的字节数据 |

### 数据获取接口 

|      功能       |                                           方法                                            |                说明                 |
| :-----------: | :-------------------------------------------------------------------------------------: | :-------------------------------: |
|   获取INT型变量值   |                  int GetIntValue(string paramName, ref int paramValue)                  | paramName：变量名称；paramValue：变量值（下同） |
|  获取Float型变量值  |               int GetFloatValue (string paramName, ref float paramValue)                |                                   |
| 获取String型变量值  |              int GetStringValue (string paramName, ref string paramValue)               |                                   |
|  获取Bytes型变量值  |               int GetBytesValue (string paramName,ref byte[] paramValue)                |                                   |
|    获取图像数据     |               int GetIMAGEValue (string paramName, ref Image paramValue)                |                                   |
|  获取INT型数组变量   |    int GetIntArrayValue(string paramName, ref int[] paramValue，out int  arrayCount)     |        arrayCount：数组个数（下同）        |
| 获取float型数组变量  |  int GetFloatArrayValue(string paramName, ref float[] paramValue，out int  arrayCount)   |                                   |
| 获取string型数组变量 | int GetStringArrayValue(string paramName, ref string[] paramValue，out int  arrrayCount) |                                   |

### 数据输出接口 

|     功能      |                                    方法                                     |                说明                 |
| :---------: | :-----------------------------------------------------------------------: | :-------------------------------: |
|  设置INT型变量值  |                  int SetIntValue(string key, int value)                   | paramName：变量名称；paramValue：变量值（下同） |
| 设置float型变量值 |                int SetFloatValue (string key, float value)                |                                   |
|  设置字符串型变量值  |               int SetStringValue (string key, string value)               |                                   |
|  设置16进制数据   |               int SetBytesValue (string key, byte[] value)                |                                   |
|   设置图像数据    |                int SetImageValue (string key, Image value)                |                                   |
|  设置字符串型数组   | int SetStringValueByIndex(string key, string value, int index, int total) |    index：数组索引；total：数组元素个数（下同）    |
|  设置Int型数组   |    int SetIntValueByIndex(string key, int value, int index, int total)    |                                   |
| 设置Float型数组  | int SetFloatValueByIndex (string key, float value, int index, int total)  |                                   |

### 调试相关

|        功能        |                方法                 |      说明      |
| :--------------: | :-------------------------------: | :----------: |
| 将信息打印至DebugView中 | void ConsoleWrite(string content) | Content：打印内容 |
|    将信息通过弹窗显示     |  void ShowMessageBox(string msg)  |   msg：弹窗内容   |

# 案例示例

## 案例1 求最值

            

```c#
using System;  
using System.Text;  
using System.Windows.Forms;  
using Script.Methods;    
public partial class UserScript:ScriptMethods,IProcessMethods  
{  
    
    int processCount ;    
    
    float Input_X;//圆心X  
	float Input_Y;    
	float MaxX;//最大值X        float MaxX; //最大值 X
	float MaxY;    

    public void Init()    
    {  

        processCount = 0;  
        //MessageBox.Show("OK");  
    
    }

    public bool Process()  
    {  
        //You can add your codes here, for realizing your desired function  
		//每次执行将进入该函数，此处添加所需的逻辑流程处理
		processCount++;    
		GetFloatValue("InputX",ref Input_X);//获取圆心X数据  
		GetFloatValue("InputY",ref Input_Y);    
		GetFloatValue("X",ref MaxX);//获取X的最值    
		GetFloatValue("Y",ref MaxY);    
		//求最值
        if(Input_X>MaxX)    
        {    
            SetFloatValue("Max_X", Input_X);    

        }
        else    
        {    
            SetFloatValue("Max_X", MaxX);    
        }
        if(Input_Y>MaxY)    
        {    
            SetFloatValue("Max_Y",Input_Y);    
        }
        else    
        {    
            SetFloatValue("Max_Y", MaxY);            
        }     
        return true;    
    }
}
                             
```
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/d98a02937d294d0c80032df5566e3c9f.png)
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/a6d3c6fa339c4d95b4b43bce0ee42370.png)                                                                                                                                                                              
## ​案例2 日志写入

```c#
using System;              
using System.Text;                
using System.Windows.Forms;              
using Script.Methods;              
using System.IO;              
public partial class UserScript:ScriptMethods,IProcessMethods              
{              

    int processCount ;               
	float Angle;              
    string writeSucceedorFailed;//写入日志是否成功              
	
    public void Init()              
    {                

        processCount = 0;              

    }

    public bool Process()              
    {              
        processCount++;              
        GetFloatValue("angle",ref Angle);               
        //结果写入日志
        try                
        {                
            using (FileStream fs = new FileStream("D:\\日志.txt", FileMode.Append, FileAccess.Write)) //打开文件流                
            {                
                byte[] buffer = Encoding.Default.GetBytes("第" + processCount + "次运行" + "Angle:" + Angle + "\r\n"); //将数据转换为字节数组                
                fs.Write(buffer, 0, buffer.Length); //写入文件                  
            }
            writeSucceedorFailed = "写入成功"; //输出日志            
        }
        catch            
        {            
            writeSucceedorFailed = "写入失败"; //输出日志            
        }

        SetStringValue("writeSucceedorFailed", writeSucceedorFailed); //输出日志到变量            

        return true;                    
    }
}
                            
```
![在这里插入图片描述](https://obsidian-xiaochao.oss-cn-shenzhen.aliyuncs.com/%E5%9B%BE%E5%BA%8A/20e12232068340d89391b37cea77cfca.png)
注意事项

# 注意事项

 - 脚本中不支持调用控制器管理里面的设备发送IO数据； 
 - 脚本模块尽量减少关于系统资源的操作，因为可能会导致重编译时候资源泄漏导致的异常，包括线程、串口端口、文件 等； 
 - 尽量只做后台业务，不涉及界面层； 
 - 尽量不在脚本中操作非托管资源，如果要操作非托管资源，则需要在process定义并且释放；
 - 脚本中的异常可以通过messagebox.show（弹框，慎用，流程运行到弹框会停止，确定后继续）来定位具体出错的位置， 然后通过try-catch语法进行捕获，并输出异常信息




