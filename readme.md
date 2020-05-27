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
    1.1. 介绍<br/>
        本文档提供用户登录会话接口说明，CP接收发货通知说明<br/>
    1.2. 声明<br/>
        1. 接口文档中涉及的APP_ID、渠道ID,APP_KEY、PAY_KEY,需要CP方向我方申请，由于涉及到加密通信，CP	方必须严格对参数进行保密。<br/>
        2. 接口文档中所有的签名编码为UTF-8,且加密结果均转换为小写字符。<br/>
        3. 只允许申请的CP方使用，严禁CP方做二次开发提供给未授权方使用。如发现违反以上声明，将追究擅自使用人的责任<br/>
        4. 本文档设计的技术均采用主流的Web开发技术，相关技术（如JSON）若不清楚，		可通过互联网搜索即可了解，本文档不做单独解释。<br/>
        5. 文档所有接口仅支持POST方式,返回信息全部为json结构<br/>
