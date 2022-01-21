---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /zh/ctrip_scraper/
title: 携程大数据-机场数据自动化爬取方式
email: contact

languages: ["zh"]
---

以下所有代码框未经注释均在terminal中运行。其中`${variable}`需替换成实际所需内容后才能执行。

# 整体趋势

样例代码

```bash
curl -A "MicroMessenger" -X POST "http://flightai-wechat.ctrip.com/api/lf/dom/trend"  
```

样例输出

```json
[
  {
    "day": "2022/02/19",
    "uv": 158444.89,
    "order": 31050.72,
    "lfp": 0,
    "cap": null,
    "uv_yoy": 267835.19,
    "order_yoy": 48626.61
  },
  ...
  {
    "day": "2022/01/21",
    "uv": 2062536.57,
    "order": 743590.85,
    "lfp": 0.5696,
    "cap": null,
    "uv_yoy": 1404522.67,
    "order_yoy": 675900.86
  }
]
```

# 机场查询

代码格式

```bash
curl -A "MicroMessenger" \
-H "Content-Type: application/json;charset=utf-8" \
-d '{"cityCode":"${AIRPORT_CODE}","type":"${TYPE_CODE}"}' \        
-X POST "http://flightai-wechat.ctrip.com/api/lf/airport/data"    
```

`AIRPORT_CODE`为机场三字代码，`TYPE_CODE`为出发到达区分码，其中出发为`1`，到达为`2`。

样例代码

```bash
curl -A "MicroMessenger" \
-H "Content-Type: application/json;charset=utf-8" \
-d '{"cityCode":"PVG","type":"1"}' \        
-X POST "http://flightai-wechat.ctrip.com/api/lf/airport/data"    
```

样例输出

```json
[
  {
    "id": 1,
    "searchindex": 403582,
    "last2week": 58847,
    "lastweek": 104878,
    "futureweek": 97863,
    "future2week": 98116,
    "future3week": 29376,
    "future4week": 14502,
    "airport": "SYX",
    "airportName": "三亚/凤凰"
  },
  ...
  {
    "id": 30,
    "searchindex": 43795,
    "last2week": 6313,
    "lastweek": 10975,
    "futureweek": 11060,
    "future2week": 11215,
    "future3week": 3071,
    "future4week": 1161,
    "airport": "LJG",
    "airportName": "丽江/三义"
  }
]
```

# 航线查询

代码格式

```bash
curl -A "MicroMessenger" \
-H "Content-Type: application/json;charset=utf-8" \
-d '{"dCityCode":"${departureAirport}","aCityCode":"${arrivalAirport}"}' \
-X POST "http://flightai-wechat.ctrip.com/api/lf/airway/prediction" 
```

`departureAirport`为出发机场三字代码，`arrivalAirport`为到达机场三字代码。

样例代码

```bash
curl -A "MicroMessenger" \
-H "Content-Type: application/json;charset=utf-8" \
-d '{"dCityCode":"SHA","aCityCode":"PEK"}' \
-X POST "http://flightai-wechat.ctrip.com/api/lf/airway/prediction"  
```

样例输出

```json
[
  {
    "day": "2022/01/25",
    "cap": 9683,
    "lf": null,
    "lfp": 0.4417,
    "lcc": 0
  },
  ...
  {
    "day": "2021/12/22",
    "cap": 8958,
    "lf": 0.677,
    "lfp": null,
    "lcc": 0
  }
]
```

<sub>
    <sup>
        (c) Tony Chen
    </sup>
</sub>
