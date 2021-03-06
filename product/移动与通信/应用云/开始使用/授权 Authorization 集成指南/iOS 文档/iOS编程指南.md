## Authorization 编程使用指南

## 开始之前



如果这是您首次向应用添加 Authorization，请完成以下步骤：

在 应用云 控制台中关联您的应用

1. 安装 [应用云 SDK]()。
2. 在 [应用云 控制台]()中，将您的应用添加到您的 应用云 项目中。
3. 参考 [Authorization 配置文档]()，配置并初始化  Authorization


## 获取 第三方 用户登录信息
### 请求 QQ 登录
> 请确保您集成并安装了 TACAuthorizationQQ 模块

首先，您需要引入头文件：

Objective-C 代码示例：
~~~
#import <TACAuthorizationQQ/TACAuthorizationQQ.h>
~~~

Swift 代码示例：
~~~
import TACAuthorizationQQ
~~~

然后调用SDK提供的requestCredential方法调起登录即可：

Objective-C 代码示例：
~~~
TACQQAuthProvider* provider = [[TACAuthoriztionService defaultService] qqCredentialProvider];
[provider requestCredential:^(TACQQCredential*credential, NSError *error) {
  //登录完成回调
  if (error) {
    TACLogError(@"ERROR %@", error);
  } else {
     //登录成功
  }
}];
~~~

Swift 代码示例：
~~~
TACCrashService.share().setUserValue("内容", forKey:"key")
let provider = TACAuthoriztionService.default().qqCredentialProvider
provider?.requestCredential({ (credential:TACQQCredential?, error:Error?)->Void in
  if error != nil{
    print("ERROR",error as Any)
    }else{
    print("登录成功")
    }
})
        
~~~
### QQ登录完成回调
登录完成回调以block的形式传入requestCredential:方法中，在请求登录后完成。
### 获取 QQ 登录用户信息
登录完成以后可以获取登录用户的信息，使用对应Provider的requestUserInfoWithCredential方法（可以在登录完成回调里去请求）：

Objective-C 代码示例：
~~~
TACQQAuthProvider* provider = [[TACAuthoriztionService defaultService] qqCredentialProvider];
[provider requestCredential:^(TACQQCredential*credential, NSError *error) {
  //登录完成回调
  if (error) {
    TACLogError(@"ERROR %@", error);
  } else {
    [provider requestUserInfoWithCredential:credential completation:^(TACOpenUserInfo *userInfo, NSError *error) {
    //获取到用户信息
    }];
    TACLogDebug(@"Credential %@", credential);
    }
}];

~~~

Swift 代码示例：
~~~
TACCrashService.share().setUserValue("内容", forKey:"key")
  let provider = TACAuthoriztionService.default().qqCredentialProvider
  provider?.requestCredential({ (credential:TACQQCredential?, error:Error?)->Void in
  if error != nil{
    print("ERROR",error as Any)
  }else{
    provider?.requestUserInfo(with: credential, completation: { (userInfo:TACOpenUserInfo?, error:Error?)->Void in
    print("credential",credential as Any)
    })
  }
})

~~~
然后您就可以使用 `TACQQAuthProvider` 来获取 QQ 的登录信息



### 请求微信登录
> 请确保您集成并安装了 TACAuthorizationWechat 模块

首先，您需要引入头文件：

Objective-C 代码示例：
~~~
#import <TACAuthorizationWechat/TACAuthorizationWechat.h>
~~~

Swift 代码示例：
~~~
import TACAuthorizationWechat
~~~
然后您就可以使用 `TACWechatAuthProvider` 来获取 微信 的登录信息

Objective-C 代码示例
~~~
TACWechatAuthProvider* provider = [[TACAuthoriztionService defaultService] wechatCredentialProvider];
[provider requestCredential:^(TACCredential *credential, NSError *error) {
  //登录完成回调
    if (error) {
        TACLogError(@"ERROR %@", error);
    } else {
      //登录成功
    }
}];
~~~

Swift 代码示例：
~~~
let provider = TACAuthoriztionService.default().wechatCredentialProvider
  provider?.requestCredential({ (credential:TACCredential?, error:Error?)->Void in
  if error != nil{
    print("ERROR",error as Any)
  }else{
     //登录成功
   }
})
    
~~~
### 微信登录完成回调
登录完成回调以block的形式传入requestCredential:方法中，在请求登录后完成。
### 获取 微信 登录用户信息
登录完成以后可以获取登录用户的信息，使用对应Provider的requestUserInfoWithCredential方法（可以在登录完成回调里去请求）：

Objective-C 代码示例
~~~
TACWechatAuthProvider* provider = [[TACAuthoriztionService defaultService] wechatCredentialProvider];
[provider requestCredential:^(TACCredential *credential, NSError *error) {
    if (error) {
        TACLogError(@"ERROR %@", error);
    } else {
        [provider requestUserInfoWithCredential:credential completation:^(TACOpenUserInfo *userInfo, NSError *error) {
          //获取登录用户的信息
        }];
        TACLogDebug(@"Credential %@", credential);
    }
}];
~~~

Swift 代码示例：
~~~
let provider = TACAuthoriztionService.default().wechatCredentialProvider
  provider?.requestCredential({ (credential:TACCredential?, error:Error?)->Void in
    if error != nil{
      print("ERROR",error as Any)
    }else{
      provider?.requestUserInfo(with: credential as! TACWechatCredential, completation: { (userInfo:TACOpenUserInfo?,     error:Error?)->Void in
        //获取登录用户的信息
      })
    }
})
~~~
