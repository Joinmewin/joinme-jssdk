# **JoinMe JSSDK for Web Readme** 



# 1.Overview

This document describes how to integrate JoinMe JSSDK in 3rd Party's Web Application. Before using JSSDK service, JSSDK authentication is required.

For authentication detail, please refer Request Access Token API which grant\_type is client\_credentials in JoinMe\_OAuth\_Readme.



# 2.Import JS

Please download JoinMe JSSDK package which contains followings files. :

**lib\jmjs\_pub.js:** JoinMe JSSDK JS file, which needs to be imported in 3rd Party Web. It supports AMD/CMD.

**example\jmjs_demo.js,  example\test.html:** Example Code, and HTML page.



# 3. Authentication Flow

Before using JSSDK, 3rd Party needs to run Authentication. Following is Authentication Flow Diagram:

![](https://raw.githubusercontent.com/Joinmewin/joinme-jssdk/master/media/jssdk01.jpg)

## 3.1 Steps Description:

**Step 1:** 3rd Party Web App calls Request Access Token API which grant\_type is client\_credentials. Please refer JoinMe\_OAuth\_Readme for detail. And then, call "Get Ticket" API to get "Ticket" for JSSDK.

**Step 2:** 3rd Party Web App calls "Set Config" to transfer ticket to JoinMe APP. Please refer "Set Config" API.

**Step 3:** JoinMe APP query identification of 3rd Party from JoinMe Server, and APP will verify if 3rd Party valid or not. If Pass, JSSDK is available for service.

## 3.2 Refresh Token:

Once Token expired, JSSDK API returns error code "103" which means "Token Expired" message. 3rd Party Web App shall run the following steps to refresh token , and ticket.

**Step 1:** 3rd Party Web App calls **Refresh** Access Token API. Please refer JoinMe\_OAuth\_Readme for detail. And then, call "Get Ticket" API to get "Ticket" for JSSDK.

**Step 2:** 3rd Party Web App calls "Set Config" to transfer ticket to JoinMe APP. Please refer "Set Config" API.

**Step 3:** JoinMe APP query identification of 3rd Party from JoinMe Server, and APP will verify if 3rd Party valid or not. If Pass, JSSDK is available for service.

# 4. API Refernece

Some APIs are not available, due to iOS limitation. Following is the support table for Android ,and iOS.



|                | Android(v1.0.24 & later) | iOS(v1.0.23& later ) |
| -------------- | ------------------------ | -------------------- |
| SetConfig      | Y                        | Y                    |
| GetQRcode      | Y                        | Y                    |
| CloseWebView   | Y                        | Y                    |
| GetLocation    | Y                        | N                    |
| GetNetworkType | Y                        | N                    |

 

## 4.1 Error Code Definition:

JSSDK API returns result by CallBack which contains Error code for detail.

| **Error Code** | **Message**           |
| -------------- | --------------------- |
| 101            | "Internal data error" |
| 102            | "Ticket is invalid"   |
| 103            | "Ticket is expired"   |
| 104            | "Url is not matched"  |
| 105            | "No public key"       |
| 106            | "JWT format error"    |
| 107            | "Please set config"   |


## 4.2 Get Ticket

3rd Party gets Ticket from JoinMe Sever , and the parameters are

1.  **Access\_Token** : This is Access Token form Request Access Token API which grant\_type is client\_credentials. Please refer JoinMe\_OAuth\_Readme for detail.

2.  **scope** : Please set "All". It's Reserved for further.

3.  **url** : JSSDK URL 3rd Party registered in JoinMe Develop Web.



**Method:**POST /access                                                                                                                                                                                                                                                                                                                                                                       **Header:**Content-Type: application/json                                                                                                                                                                                                                                                                                                                                                     **Authorization:** Bearer {{**access\_token**}}                                                                                                                                                                                                                                                                                                                                        **Request:**  

```json
//Get Ticket
{
    "query":"query {jsSdkScope {ticket(scope: \"All\", url: \"https://www.3rdParty.com/\")}}"
}

```

**Response:**

```json
{
    "resp": {
        "data": {
            "jsSdkScope": {
                "ticket": "eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NiJ9.eyJleHAiOjE1MTA4MDYzNTUsImlhdCI6MTUxMDc5OTE1NSwidGlja2V0SWQiOiJQQzdlb05uQTVaWXNVWGozaHFVRCIsInVybCI6Imh0dHBzOi8vd3d3Lmdvb2dsZS5jb20udHcvIiwiRnVuY3Rpb25MaXN0IjoiQWxsIiwiYWdpZCI6Ijk3NzQ2MWZlLWI5N2UtNGVjZi05N2QwLTI3M2U4MGQzYmNlYyJ9.HwiUsWPJLJyHcV4r1-kNOvQhbS0748ssKxIcetX_xKB9Ns6hQnSvYP_Lmuxr2tznb9XfS0f0Ij8ooS2QZjI9Yw"
            }
        }
    }
}
```



## 4.3 SetConfig

Once 3rd Party gets Ticket, you need to transfer it to JoinMe APP by this API.

```javascript
    var SetConfig = function(){
        var params = {

            jwt :"eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NiJ9.eyJleHAiOjE1MTA3MzEyMjQsImlhdCI6MTUxMDcyNDAyNCwidGlja2V0SWQiOiJVNGlWMzJFTHhuU1NEU2ZsVVFGeiIsInVybCI6Imh0dHBzOi8vd3d3Lmdvb2dsZS5jb20udHcvIiwiRnVuY3Rpb25MaXN0IjoiQWxsIiwiYWdpZCI6ImI0YjA0YTNiLWY5MWItNDU1Yy05N2QwLWJlYWExZGNjMzU0ZCJ9.siQvRlawE3lAlh58MV9XWxigs3yZ9XzrFUk2A42qxozUNG9wVlRm4MAQtBdS69grO297beFdarJ4K2UYfkfEBw",

            cb:function(err,res){
                if(err)
                    alert(res);
                else
                    alert(res);
            }
        };
        jmjs.SetConfig(params);
    }
```



## 4.4 GetLocation

Return User's Location Information.

```javascript
   var GetLocation = function(){
        var params = {
            cb:function(err,res){
                if(err)
                    alert(res);
                else{
                      alert(res.latitude);
                      alert(res.longitude);
                }
            }
        };
        jmjs.GetLocation(params);
    }
```

**Return:** 

​	res.latitude : 緯度 

​	res.longitude : 經度



## 4.5 GetNetworkType

Return user's network.

```
    var GetNetworkType = function(){
        var params = {
            cb:function(err,res){
                if(err)
                    alert(res);
                else
                    alert(res);
            }
        };
        jmjs.GetNetworkType(params);
    }
```


**Return:**                         

​    res //return type is String, ether of following values 

​    //"WIFI"              

​    //"MOBILE"    // Data Network

​    //"No network"  



## 4.6 GetQRcode

Return user's network.

```javascript
    var GetQRcode = function(){
        var params = {
            cb:function(err,res){
                if(err)
                    alert(res);
                else
                    alert(res);
            }
        };
        jmjs.GetQRcode(params);
    }
```

**Return:**
  res //String value                 

​        // e.g. "https://www.3rdparty.com" 



## 4.7 CloseWebView

Close current web view window.

```javascript
    var CloseWebView = function(){
        var params = {
            cb:function(err,res){
                if(err)
                    alert(res);
                else
                    alert(res);
            }
        };
        jmjs.CloseWebView(params);
    }
```



# 5.License

[Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0) 