## 乐众移动游戏SDK服务端接入协议
  - [1.简介](#introduce) 
      - [1.1介绍](#introduce)
      - [1.2声明](#announce)

  - [2.登录验证](#checklogin)
      - [2.1接口地址](#loginapiurl)
      - [2.2请求方式](#loginapimethod)
      - [2.3请求参数](#loginapiparam)
      - [2.4签名方式](#loginsignmethod)
      - [2.5返回值](#loginreturn)

  - [3.发货通知](#paynotice)
      - [3.1接口地址](#payapiurl)
      - [3.2请求方式](#payapimethod)
      - [3.3请求参数](#payapiparam)
      - [3.4签名方式](#paysignmethod)
      - [3.5返回值](#payreturn)
      - [3.6发货接口说明](#paynoticeintro)

  - [4.附录](#appendix)
      - [4.1加密算法示例（php）](#phpcode)
      
  <h3 id="introduce" style="display:none;"> 1.简介 </h3> 
  
  &ensp;&ensp; **1.1. 介绍** <br/>
        &ensp;&ensp;&ensp;&ensp;本文档提供用户登录会话接口说明，CP接收发货通知说明<br/>
  &ensp;&ensp; **1.2. 声明** <br/>
        &ensp;&ensp;&ensp;&ensp;1. 接口文档中涉及的APP_ID、渠道ID,APP_KEY、PAY_KEY,需要CP方向我方申请，由于涉及到加密通信，CP	方必须严格对参数进行保密。<br/>
        &ensp;&ensp;&ensp;&ensp;2. 接口文档中所有的签名编码为UTF-8,且加密结果均转换为小写字符。<br/>
        &ensp;&ensp;&ensp;&ensp;3. 只允许申请的CP方使用，严禁CP方做二次开发提供给未授权方使用。如发现违反以上声明，将追究擅自使用人的责任<br/>
        &ensp;&ensp;&ensp;&ensp;4. 本文档设计的技术均采用主流的Web开发技术，相关技术（如JSON）若不清楚，		可通过互联网搜索即可了解，本文档不做单独解释。<br/>
        &ensp;&ensp;&ensp;&ensp;5. 文档所有接口仅支持POST方式,返回信息全部为json结构<br/>
        
   <h3 id="checklogin" style="display:none;"> 2.登录验证 </h3> 

   &ensp;&ensp; **2.1. 登录地址** <br/>
   
   | 地址类型   |      地址      |
   |----------|:-------------:|
   | 正式地址 |  https://lzapi.lezhonggame.com/login/AuthToken |
   
   &ensp;&ensp; **2.2. 请求方式** <br/>
   &ensp;&ensp; &ensp;&ensp;**POST**<br/>
   
   &ensp;&ensp; **2.3. 请求参数** <br/>
   | 参数名   |      参数规格      | 必填   |      说明      |
   |----------|:-------------:|:-------------:|:-------------:|
   | channel_pkg_num |  Int |  Y |  渠道ID |
   | token |  String |  Y |  客户端SDK返回的验证口令 |
   | time |  Int |  Y |  当前时间 |
   | sign |  String |  Y |  签名 |
   
   &ensp;&ensp; &ensp;&ensp; ***注意：以上字段请求时，都无需urlencode***
   
   &ensp;&ensp; **2.4. 签名方式** <br/>
   &ensp;&ensp;&ensp;&ensp;除去sign,按键值排序拼串，token值要进行urlencode, 然后拼接&{app_key},例如
md5("channel_pkg_num=88001&time=1498878255&token=ddh24e23cdscjwe8fdse328rs&" + {app_key}),其中+时连接符

  &ensp;&ensp; **2.5. 返回值** <br/>
  | 参数名   |      参数规格      | 必填   |      说明      |
   |----------|:-------------:|:-------------:|:-------------:|
   | code |  Int |  Y |  状态码 0为成功，其它标识失败 |
   | msg |  String |  Y |  code=0，返回账号信息，例如{"uid":"VPH59531C692B132"}code不为0时，返回纯字符串，例如验签错误： sign error! |
   
   <h3 id="paynotice" style="display:none;"> 3.发货通知 </h3>
   
   &ensp;&ensp; **3.1. 接口地址** <br/>
   | 地址类型   |      地址      |
   |----------|:-------------:|
   | 正式地址 |  由我方后台配置或CP客户端传入SDK客户端，优先我方后台配置<br/>通知地址必须以http(s)://开头<br/>正确示例：http(s)://www.XXX.com/recharge <br/>错误示例：www.XXX.com/recharge<br/>注：回调地址中不能存在&符号 |
