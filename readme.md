## 乐众移动游戏SDK服务端接入协议
  - [1.简介](#introduce) 
      - [1.1介绍](#intro1)
      - [1.2声明](#intro2)

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
  
   <h4 id="intro1" style="display:none;"> 1.1 介绍 </h4>
        &ensp;&ensp;&ensp;&ensp;本文档提供用户登录会话接口说明，CP接收发货通知说明<br/>
   <h4 id="intro2" style="display:none;"> 1.2 声明 </h4>
        &ensp;&ensp;&ensp;&ensp;1. 接口文档中涉及的APP_ID、渠道ID,APP_KEY、PAY_KEY,需要CP方向我方申请，由于涉及到加密通信，CP	方必须严格对参数进行保密。<br/>
        &ensp;&ensp;&ensp;&ensp;2. 接口文档中所有的签名编码为UTF-8,且加密结果均转换为小写字符。<br/>
        &ensp;&ensp;&ensp;&ensp;3. 只允许申请的CP方使用，严禁CP方做二次开发提供给未授权方使用。如发现违反以上声明，将追究擅自使用人的责任<br/>
        &ensp;&ensp;&ensp;&ensp;4. 本文档设计的技术均采用主流的Web开发技术，相关技术（如JSON）若不清楚，		可通过互联网搜索即可了解，本文档不做单独解释。<br/>
        &ensp;&ensp;&ensp;&ensp;5. 文档所有接口仅支持POST方式,返回信息全部为json结构<br/>
        
   <h3 id="checklogin" style="display:none;"> 2.登录验证 </h3> 

   <h4 id="loginapiurl" style="display:none;"> 2.1. 登录地址 </h4>
   
   | 地址类型   |      地址      |
   |----------|:-------------:|
   | 正式地址 |  https://lzapi.lezhonggame.com/login/AuthToken |
   
   <h4 id="loginapimethod" style="display:none;">2.2. 请求方式 </h4>
   
   &ensp;&ensp; &ensp;&ensp;**POST**<br/>
   
   <h4 id="loginapiparam" style="display:none;">2.3. 请求参数 </h4>
   
   | 参数名   |      参数规格      | 必填   |      说明      |
   |----------|:-------------:|:-------------:|:-------------:|
   | channel_pkg_num |  int |  Y |  渠道ID |
   | token |  string |  Y |  客户端SDK返回的验证口令 |
   | time |  int |  Y |  当前时间 |
   | sign |  string |  Y |  签名 |
   
   &ensp;&ensp; &ensp;&ensp; ***注意：以上字段请求时，都无需urlencode***
   
   <h4 id="loginsignmethod" style="display:none;">2.4. 签名方式 </h4>
   &ensp;&ensp;&ensp;&ensp;除去sign,按键值排序拼串，token值要进行urlencode, 然后拼接&{app_key},例如
md5("channel_pkg_num=88001&time=1498878255&token=ddh24e23cdscjwe8fdse328rs&" + {app_key}),其中+时连接符

  <h4 id="loginreturn" style="display:none;">2.5. 返回值 </h4>
  
  | 参数名   |      参数规格      | 必填   |      说明      |
   |----------|:-------------:|:-------------:|:-------------:|
   | code |  int |  Y |  状态码 0为成功，其它标识失败 |
   | msg |  string |  Y |  code=0，返回账号信息，例如{"uid":"80000000240","channel_id":3,"sdk_uid":"20396935"},code不为0时，返回纯字符串，例如验签错误： sign error! |
   
&emsp;&emsp;**说明**<br/>
&emsp;&emsp;&emsp;&emsp;uid:CP作为用户标识,存字符串<br/>
&emsp;&emsp;&emsp;&emsp;channel_id:子渠道ID<br/>
&emsp;&emsp;&emsp;&emsp;sdk_uid:渠道用户标识（注意：这是子渠道的用户标识，CP可记录，切不可把此值作为用户标识）<br/>
   
   <h3 id="paynotice" style="display:none;"> 3.发货通知 </h3>
   
   <h4 id="payapiurl" style="display:none;">3.1. 接口地址 </h4>
   
   | 地址类型   |      地址      |
   |----------|:-------------:|
   | 正式地址 |  由我方后台配置或CP客户端传入SDK客户端，优先我方后台配置<br/>通知地址必须以http(s)://开头<br/>正确示例：https://www.XXX.com/recharge <br/>错误示例：www.XXX.com/recharge<br/>***注：回调地址中不能存在&符号***</span> |
   
   <h4 id="payapimethod" style="display:none;">3.2. 请求方式 </h4>
   
   &ensp;&ensp; &ensp;&ensp;**POST**<br/>
   
   <h4 id="payapiparam" style="display:none;">3.3. 请求参数 </h4>
   
   | 参数名   |      参数规格      | 必填   |      说明      |
   |----------|:-------------:|:-------------:|:-------------:|
   | channel_pkg_num |  int |  Y |  渠道ID |
   | my_order_num |  string |  Y |  LZ订单号 |
   | cp_order_num |  string |  Y |  CP订单号 |
   | extra |  string |  Y |  透传参数,CP未传，则以空字符串加密，不要使用json格式，建议使用param1_param2_param3，以下划线分隔的字符串 |
   | role_id |  string |  Y |  角色ID |
   | role_name |  string |  Y |  角色名 |
   | product_num |  string |  Y |  产品ID 如果是通过此字段来确定发货，注意要较验对应的金额 |
   | product_name |  string |  Y |  产品名称 |
   | server_id |  string |  Y |  服务器ID |
   | server_name |  string |  Y |  服务器名称 |
   | currency |  string |  Y |  币种：RMB: 人民币USD:  美元TWD: 台币THB : 泰铢...... |
   | amount |  string |  Y |  充值金额（单位：分），发货前金额需要验证 |
   | pay_result |  string |  Y |  支付结果：1.成功 2 失败 |
   | sign |  string |  Y |  签名 |
   
   <h4 id="paysignmethod" style="display:none;">3.4. 签名方式 </h4>
   &ensp;&ensp;&ensp;&ensp;除去sign,并且为空（null）的值不参与拼串，按键值排序拼串，然后拼接&{pay_key}，和登陆的签名类似
   
   <h4 id="payreturn" style="display:none;">3.5. 返回值 </h4>
   &ensp;&ensp;&ensp;&ensp;CP方接收回调后，返回SUCCESS(大写,不带引号，前后不能带空格)表示成功，其余例如FAIL表示失败！
   
   <h4 id="paynoticeintro" style="display:none;">3.6. 发货接口说明 </h4>
     &ensp;&ensp;&ensp;&ensp;1.SDK服务端系统订单会在支付完成之后实时通知到CP发货接口<br/>
     &ensp;&ensp;&ensp;&ensp;2.SDK服务端系统通知CP发货时，若未收到CP接口成功回复，该订单将重复通知3次。<br/>
     &ensp;&ensp;&ensp;&ensp;3.SDK服务端行在3次重复通知CP发货的过程中，都未收到CP的成功回复，订单将进入后台轮询通知发货。<br/>
     &ensp;&ensp;&ensp;&ensp;4.SDK服务端系统若收到CP接口返回成功（SUCCESS）,将不再对该笔订单做重复通知。<br/>
     &ensp;&ensp;&ensp;&ensp;5.SDK服务端系统若对某笔订单重复通知，则CP需自行判断是否已经发货，若已发货，则直接返回成功（SUCCESS）,当做成功处理。<br/>
     
   <h3 id="appendix" style="display:none;"> 4.附录 </h3>
   
   <h4 id="phpcode" style="display:none;">4.1.加密算法示例 </h4>
   
```php
function getSign($param, $key)
{
    $str = '';
    ksort($param);
    foreach($param as $k => $v)
    {
      if (is_null($v))
        continue;
       $str .= $k . '=' . urlencode($v) . '&';
    }
    return md5($str .  $key);
}
```
   
