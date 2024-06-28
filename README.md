<p align="center">
  <a href="http://moca-tech.net/">
    <img src="https://www.moca-tech.net/logo.png" alt="S2S" height=72>
  </a>
  <h3 align="center">Moca Tech</h3>
  <p align="center">
    S2S广告推广同步接口.
    <br>
    zh
    ·
    <a href="https://github.com/moca-tech/Performance-S2S-API/tree/master/en" target="_blank">en</a>
    <br>
    <a href="http://www.moca-tech.net" target="_blank"><strong>More »</strong></a>
    <br>
    <br>
    <a href="mailto:business@moca-tech.net">Business@moca-tech.net</a>
  </p>






## 使用说明

调用接口前请仔细阅读参数使用说明,常见问题如下：

1.	获取价格(payout)时，请同时获取币种(currency)。如您的系统只支持单一币种，比如美元，请在对接时通知您的AM，避免设置成多币种造成混乱；

3.	确认返回offer是否有效时,请检查remaining>0，否则代表当日cap跑满；

4.	Daily cap根据广告主设置的时区控制。Daily cap会在广告主指定的时区00：00分重置，重置后因超额而暂停的offer会自动重启。


## 请求

1. 请求接口：http://s2s.moca-tech.net/v2.0/sync
2. 请求方式：GET|POST
3. 请求频率：建议每10分钟一次
4. 注意事项：项目质量要求，时区
5. 请求参数

| 参数名   | 类型   | 必要性 | 说明                                                         |
| -------- | ------ | ------ | ------------------------------------------------------------ |
| aff_id   | int    | 是     | 渠道ID，示例：10001                                          |
| apikey   | string | 是     | 访问授权密钥（需向AM申请，可重置）                           |
| country  | string | 否     | 指定国家CODE，默认为ALL。<br />多个国家用“，”分隔            |
| platform | string | 否     | 操作系统，默认为ALL。可选项[IOS\|ANDROID]。<br />多个用“，”分隔 |
| device   | string | 否     | 设备类型，默认为ALL。可选项目[MOBILE\|PC]。<br />多个用“，”分隔 |
| traffic  | int    | 否     | 激励类型，默认为2（不限）。<br />可选项：<br />0 - 非激励<br />1- 激励<br />2 - 不限 |
| offer_id | int    | 否     | 指定获取某个Offer信息                                        |

6. 示例

   > http://s2s.moca-tech.net/v2.0/sync?aff_id=10001&country=IN,CN&platform=ANDROID,IOS&device=mobile,pc&apikey=[apikey]

## 响应

1. S2S Response: JSON

| 字段名                                      | 类型          | 说明                                                         |
| ------------------------------------------- | ------------- | ------------------------------------------------------------ |
| result &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | boolean       | 处理结果[true\|false]                                        |
| info                                        | string        | 处理结果描述                                                 |
| number                                      | int           | 返回可推广的项目数量                                         |
| list                                        | array[object] | 广告项目列表<br />注意广告下架、暂停、超额等，不会出现在列表中，建议暂停投放。 |

2. 广告结构

| 字段名                                                       | 类型                                                     | 说明        |
| ------------------------------------------------------------ | ------------------------------------------------------------ | --------------- |
| offer_id | int | 广告项目ID（广告项目需要向AM申请） |
| offer_name | string | 广告项目名称 |
| adtype | string | 广告类型[APP\|Others]                                        |
| traffic | boolean | 是否允许激励流量[true\|false] |
| countries | string | 指定投放的国家Code，ALL或空值为不限国家，多个以“\|”分隔。<br />如： “IN\|CN” |
| states | string | 指定投放的邦名称，ALL或空值为不限，多个以“\|”分隔。<br />如：“Maharashtra“ |
| cities | string | 指定投放的城市，ALL或空值为不限城市，多个以“\“分隔。<br />如：“Bengaluru” |
| carriers                                               | string                                                  | 指定投放的运营商，ALL或空值为不限运营商，多个以“\|”分隔。<br />如：“Airtel\|Jio" |
| device | string | 指定投放的设备类型，ALL或空值为不限设备，多个以“\“分隔。<br />如：“MOBILE“ |
| platform | string | 指定投放的操作系统[ANDROID\|IOS] |
| min_version | string | 指定投放的系统版本最低要求，空为不限 |
| quality_description | string | 质量要求描述信息 |
| quality_on | string | 质量考核指标<br />0 - 无质量要求<br />1 - Register<br />2 - Search<br />3 - Purchase<br />4 - Add to Cart<br />5 - Listen Song<br />6 - Ask Question<br />7 - Video Watch<br />8 - Login<br />9 - Successful Reply<br />12 - Homepage/Open APP<br />17 - First Trip<br />18 - Load Money<br />21 - Subscription<br />25 - International flight booking<br />26 - Domestic flight booking |
| quality_rate | string | 质量要求比例，如：15% |
| quality_type | int | 0 - None<br />1 - Event Ratio<br />2 - Unique Event Ratio<br />91 - 1st Day Retention<br />92 - 2nd Day Retention<br />97 - 7th Day Retention<br />915 - 15th Day Retention<br />930 - 30th DayRetention |
| paymode | string | 广告计费方式[CPI\|CPR\|CPS\|CPA\|SUB\|DDL\|APK\|PPC] |
| payout | float | 单价 |
| currency | string | 货币单位，如：“USD” |
| time_zone | string | 项目投放时区<br />如：“5.5” 指GMT +5.5，为印度时区 |
| dialy_caps | int | 渠道每日额度 |
| remaining | int | 当日广告项目剩余Cap，建议根据广告剩余Cap结合转化率决定投放量。 |
| device_id_mandatory | boolean | GAID/IDFA是否必传 |
| preview_link | string | 广告项目预览地址 |
| tracking_link | string | 广告项目追踪链接，参数如下（以下参数亦可用于Postback回传）：<br />aff_pub：子渠道或广告位<br />aff_sub：渠道点击ID（Transaction ID），通常用于Postback供媒体方归因<br />gaid：Google Advertising ID， Android平台上传<br />idfa: Apple IDFA，iOS平台上传<br />app_bundle：App流量包名<br />app_name：App流量名字<br />渠道自定义参数：aff_sub2, aff_sub3, aff_sub4, aff_sub5 |
| link_type | string | 追踪链接类型[WEB\|MARKET] |
| imp_tracker | string | 广告项目展示上报链接（上传参数参照tracking link）<br />注意：当device_id_mandatory为true时，需要携带gaid或idfa |
| rating | float | appstore评分 |
| package_name | string | APP包名 |
| title | string | 推广标题 |
| description | string | 推广内容或商品描述 |
| icon | string | 广告图标 |
| creative_list | arary[object] | 素材列表，包含URL、格式、尺寸 |
| creative | string | 素材压缩包下载地址 |
| sub_blacklist | array[string] | 字渠道黑名单，需要暂停的子渠道 |
| sub_whiltelist | array[string] | 字渠道白名单，建议加量的子渠道 |

3. 示例

```json
{
  "result": true,
  "info": "OK",
  "number": 1,
  "list": [
    {
      "min_version": "",
      "device_id_mandatory": true,
      "quality_type": 0,
      "icon": "https://lh3.googleusercontent.com/c0uMGe9aLRQxGtAXB_9xnKnG83VOGRD7NtglvnzPj9SYS3_ZYA_EYhtP86nePn0vjI0=s180-rw",
      "payout": 0.3,
      "rating": 0,
      "description": "We’re changing the way you find and play the music you love. Listen free with a Prime membership or get more with Amazon Music Unlimited.",
      "title": "Prime Music_Af",
      "platform": "ANDROID",
      "sub_whitelist": [ ],
      "states": "",
      "sub_blacklist": [
        "a",
        "b",
        "c"
      ],
      "preview_link": "https://play.google.com/store/apps/details?id=com.amazon.mp3",
      "currency": "USD",
      "quality_rate": "0.00%",
      "tracking_link": "http://tracking.moca-tech.net/go2offer?offer_id=105850&aff_id=10001&bid=0.3",
      "traffic": false,
      "adtype": "APP",
      "cities": "",
      "carriers": "ALL",
      "quality_description": "",
      "countries": "IN",
      "time_zone": 0,
      "creative": "http://s3-ap-southeast-1.amazonaws.com/cactiflos/ad/201908/07d3b5b3b2476aa068eb.zip",
      "offer_id": 10000,
      "offer_name": "10000-Prime Music_Af-CPI-Non Incent-Android(IN)",
      "remaining": 5000,
      "paymodel": "CPI",
      "link_type": "MARKET",
      "quality_on": "",
      "imp_tracker": "",
      "creative_list": [ ],
      "package_name": "com.amazon.mp3",
      "daily_caps": 5000,
      "device": "MOBILE"
    }
  ]
}
```
