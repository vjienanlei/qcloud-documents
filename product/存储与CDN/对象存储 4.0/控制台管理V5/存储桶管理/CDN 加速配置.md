## 简介

CDN 加速适用于对 COS 存储桶中的内容进行下载、分发，特别是对于相同内容反复下载的使用场景。

## 设置说明

在域名已开启了 CDN 加速的情况下，如果存储桶作为源站设置为公有读，那么系统会默认用户可以直接通过 CDN 加速域名或 COS 源站域名访问到源站中的对象。如果存储桶作为源站设置为私有读，用户要对数据安全进行相关配置，则需要用户开启 CDN 回源鉴权和 CDN 鉴权配置两个选项。

- 回源鉴权（开启前提是已添加 CDN 服务授权）：当用户请求的数据在边缘节点没有命中缓存时，CDN 需要回源获取数据内容。使用 COS 作为源站并开启回源鉴权后，CDN 边缘节点将使用特殊的服务身份（需要通过 CDN 服务授权得到此身份）访问 COS 源站，以实现获取并缓存私有访问存储桶中的数据。
- CDN 鉴权配置：当用户通过访问边缘节点获取缓存数据的时候，边缘节点会根据鉴权配置规则，校验访问 URL 中的身份验证字段，以此防范非授权的访问，实现防盗链，提高边缘节点缓存数据的安全性和可靠性。

CDN 鉴权配置和 CDN 回源鉴权的使用情况不冲突，但是分别配置不同的状态对数据的保护不同的使用效果，具体情况如下表：

| 存储桶访问权限      | 是否开启 CDN 回源鉴权 | 是否开启 CDN 鉴权配置 | 通过 CDN 加速域名是否可访问源站  |通过 COS 源站域名是否可访问源站   | 适用场景         |
| ------------------- | ------------ | ------------ | --------------- | --------------- | ------------ |
| 公有读              | 关闭         | 关闭         | 可访问          | 可访问          | 全站公共访问 |
| 公有读              | 关闭         | 开启         | 需使用 URL 鉴权 | 可访问          | 不推荐       |
| 公有读              | 开启         | 关闭         | 不可访问        | 可访问          | 不推荐       |
| 公有读              | 开启         | 开启         | 需使用 URL 鉴权 | 可访问          | 不推荐       |
| 私有读+CDN 服务授权 | 开启         | 开启         | 需使用 URL 鉴权 | 需使用 COS 鉴权 | 全链路保护   |
| 私有读+CDN 服务授权 | 关闭         | 开启         | 需使用 URL 鉴权 | 需使用 COS 鉴权 | 不推荐       |
| 私有读+CDN 服务授权 | 开启         | 关闭         | 可访问          | 需使用 COS 鉴权 |源站保护     |
| 私有读+CDN 服务授权 | 关闭         | 关闭         | 不可访问        | 需使用 COS 鉴权 | 不推荐       |
| 私有读              |              |              | 不可访问        | 需使用 COS 鉴权 | 无法使用 CDN |

> 注意：以第一行为例，当源站存储桶访问权限为公有读的时候，如果既不开启 CDN 回源鉴权也不开启 CDN 鉴权配置，那么通过 CDN 域名可以直接访问 CDN 边缘节点和源站存储桶，通过 COS 域名可以直接访问源站存储桶。

## 默认加速域名
### 设置步骤
#### 默认 CDN 加速
1. 单击存储桶详情界面上方的【域名管理】，点击【编辑】，把默认加速域名的当前状态设置为开启。源站类型通常默认为 XML 节点，如果作为源站的存储桶开启了静态网站，并且希望为静态网站加速，则选择为静态网站节点。
![开启默认域名](https://main.qcloudimg.com/raw/64cd680bb9d3e03fee7dde5c05f9d3fa.png)
> 注意：
> 如果用户在之前从未使用过腾讯云 CDN 服务，则无法进入【域名管理】，需先进入 CDN 控制台使用 CDN 服务。
2. 当存储桶为私有读时，需要开启 **回源鉴权** （在已添加 CDN 服务授权前提下）；当存储桶为公有读时，则不需要开启 **回源鉴权**，最后点击【保存】按钮即可开启 CDN 加速。
![CDN 授权](https://main.qcloudimg.com/raw/26d4f635553406d85f7e99a2fe87e3f9.png)

#### 开启 CDN 鉴权
1. 对于允许公有读的存储桶，CDN 边缘节点无需获得授权即可对存储桶进行访问，故也无需开启回源鉴权。对于设置为私有读的存储桶，需要开启回源鉴权来识别 CDN 边缘节点的服务身份，通过身份验证的 CDN 边缘节点才能对存储桶中对象数据进行操作。如果未添加 CDN 授权是无法开启回源鉴权的，请返回第二步添加授权，在第二步添加 CDN 服务授权之后，开启【回源鉴权】，并点击【保存】即可开启 CDN 加速。
![开启回源鉴权](https://main.qcloudimg.com/raw/9bf3808c008374ad97d2de31264a2b9c.png)
> 注意：对于私有读存储桶，同时开启回源鉴权和 CDN 服务授权会导致通过 CDN 访问源站时无需携带签名，CDN 缓存资源会进行公网分发，导致数据的安全性受到影响，建议开启 CDN 鉴权。

2. 点击【保存】之后，“默认加速域名”下会出现 CDN 鉴权状态提示，对于隐私要求严格的数据，一定要开启 CDN 鉴权，可通过页面上【鉴权配置】直接跳转到对应域名的 CDN 安全配置页面进行配置，也可以通过 CDN 控制台进入，路径为【域名管理】>>对应域名的【管理】>>【安全配置】。
![鉴权配置](https://main.qcloudimg.com/raw/6037e9e47562a913fff6c13a8ab79cb2.png)

3. 点击“鉴权配置”旁边的按钮开启功能。
![开启鉴权配置](https://main.qcloudimg.com/raw/a04220ffdd7498df3ec6319c090b40af.png)

4. 鉴权 Key 可以自己设置也可以随机生成，要求是 6-32 个由大小写字母或数字组成的字符串。有效时间是鉴权 URL 能够生效的时长，由用户自己设置。设置完成之后点击【确定】。
![详细鉴权配置](https://main.qcloudimg.com/raw/88fdc821546ad36b905cc0f350e6dadc.png)

5. 点击【鉴权计算器】，如果上一步已经配置好，鉴权 Key 和有效时间会自动填入，通常只用输入需要访问的对象的 Path，然后点击【生成】即可得到鉴权 URL，使用鉴权 URL 能直接访问到目标对象。
![鉴权计算器标志](https://main.qcloudimg.com/raw/7362294fd7bea6ff2e28843a996740c2.png)
![鉴权计算器](https://main.qcloudimg.com/raw/c3dd941a4100d39d7c093636b5ff40e3.png)
## 自定义加速域名
### 设置步骤

> 注意：此处仅介绍从 COS 控制台完成自定义域名添加及开启 CDN 加速，若要从 CDN 控制台添加自定义域名，可以参考 [域名接入](https://cloud.tencent.com/document/product/228/5734)。 

1. 登录 [对象存储控制台](https://console.cloud.tencent.com/cos5) ，进入左侧菜单栏【存储桶列表】，单击需要配置域名的存储桶（如 example），进入存储桶。 
![存储桶界面](https://main.qcloudimg.com/raw/1ad8ad5a4a70ec21763fe313bf53bdfa.png)

2. 从存储桶页面上方进入【域名管理】，在第二栏“自定义加速域名”处单击【添加域名】，输入待绑定的自定义域名（如 ` www.example.com`），选择开启回源鉴权，单击右侧的【保存】即可完成域名添加。
![自定义域名](https://main.qcloudimg.com/raw/cfda48bf50a39ce1a82ff9bf1fcf6940.png)
> 注意：对于私有读存储桶，同时开启回源鉴权和 CDN 服务授权会导致通过 CDN 访问源站时无需携带签名，CDN 缓存资源会进行公网分发，导致数据的安全性受到影响，建议开启 CDN 鉴权，进入步骤三。

3. 保存自定义域名之后，CDN 鉴权栏会出现 CDN 鉴权功能开关，可开启自定义域名 CDN 鉴权。
4. 登陆 [CDN 控制台](https://console.cloud.tencent.com/cdn/access)，进入左侧菜单栏【域名管理】，点击要配置的域名的【管理】，选择【安全配置】。鉴权 Key 可以自己设置也可以随机生成，要求是 6-32 个由大小写字母或数字组成的字符串。有效时间是鉴权 URL 能够生效的时长，由用户自己设置。设置完成之后点击【确定】。
![详细鉴权配置](https://main.qcloudimg.com/raw/88fdc821546ad36b905cc0f350e6dadc.png)

5. 点击【鉴权计算器】，如果已经配置好鉴权，鉴权 Key 和有效时间会自动填入，通常只用输入需要访问的对象的 Path，然后点击【生成】即可得到鉴权 URL，使用鉴权 URL 能直接访问到目标对象。
![鉴权计算器标志](https://main.qcloudimg.com/raw/7362294fd7bea6ff2e28843a996740c2.png)
![鉴权计算器](https://main.qcloudimg.com/raw/c3dd941a4100d39d7c093636b5ff40e3.png)

## 注意事项

用户为域名启用 CDN 加速之后，任何人都可以通过此域名直接访问源站，所以如果您的数据有一定的私密性，请您务必通过 **鉴权配置** 来保护您的源站数据。
