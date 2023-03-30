## 简介

因为一直很想找到一个符合自己 xp 的色图 api，所以干脆动手给自己写了一个。(滑稽)

大部分图片主要以萝莉，少女，可爱风格为主，偶尔会出现一些巨乳或者御姐类的作品，但并不会包含如 ~~R18G~~ 或者 ~~男の娘~~ 等超出作者 XP 范围外的图

参考了 [神代綺凛 的 Lolicon Api](https://api.lolicon.app) 的参数和返回格式，增加了 level 参数，用于区别图片的社保程度

因为服务器在港区，所以请求响应的速度会相对较慢一点

?> 本 API 仅储存了作品的基本信息，并不提供图片的代理或储存服务。

!> 所有图片均来自 Pixiv，图片版权归其创作者所有。

!> 使用本 Api 是你个人的自愿行为，使用本 api 所造成的一切风险和不良后果均与本人无关

!> **域名 lolisuki.cc 将会在2023-05-10到期，api已经迁移到了新域名 [lolisuki.cn](https://lolisuki.cn)**

## 请求

```http
GET https://lolisuki.cn/api/setu/v1
```

```http
POST https://lolisuki.cn/api/setu/v1
Content-Type: application/json
```

## 参数

| 参数名  |  数据类型  |    默认值    | 说明                                                                 |
| ------- | :--------: | :----------: | -------------------------------------------------------------------- |
| `ai`    |   `int`    |     `2`      | 是否包含 AI作品，`0`为不包含，`1`为只包含 AI，`2`为包括 AI 和非AI    |
| `r18`   |   `int`    |     `0`      | 是否包含 R-18 ，`0`为不包含，`1`为只包含 R18，`2`为包括 R18 和 非R18 |
| `num`   |   `int`    |     `1`      | 返回色图的数量，范围为 1~5                                           |
| `uid`   |  `int[]`   |              | 指定画师 id                                                          |
| `tag`   | `string[]` |              | 指定作品标签，[详见下文](#tag)                                       |
| `proxy` |  `string`  | `i.pixiv.re` | 图片代理服务                                                         |
| `level` |  `string`  |    `0-3`     | 图片的社保程度，范围为 0~6，[详见下文](#level)                       |
| `taste` |  `string`  |     `0`      | 图片类型，多个类型之间用逗号隔开，[详见下文](#taste)                 |
| `full`  |   `int`    |     `0`      | 匹配 Tag 的方式，`0`为部分一致，`1`为完全一致                        |

### tag

Get 方式请求时，Tag 参数可以通过`&`符号连接多个。Tag 之间用`|`符号隔开时，代表只需要匹配其中任意一个

例如想要查找（萝莉或者少女) 并且带有 (白丝或者黑丝) 的色图可以这样发送请求

```http
Get https://lolisuki.cn/api/setu/v1?tag=萝莉|少女&tag=白丝|黑丝
```

```http
Post https://lolisuki.cn/api/setu/v1
Content-Type: application/json
{
  "tag": [
    "萝莉|少女",
    "白丝|黑丝"
  ]
}
```

### level

这是一个根据个人 xp 整理出来的参数，仅代表我个人认为一张色图的社保程度

- 0：除了好看以外没什么特别之处
- 1：好看，也有点涩
- 2：涩
- 3：很涩
- 4：R18擦边球
- 5：R18
- 6：R18+有氧模式

假如想要查询 level0-level6 的作品，参数值可以填`0-6`

假如只想查询 level3 的作品，参数值可以填`3`

### taste

图片类型，可以同时指定多个类型，多个类型之间用逗号隔开

- 0：随机
- 1：萝莉
- 2：少女
- 3：御姐

假如想要查询 `萝莉或者少女` 类型的作品，参数值可以填`1,2`

假如只想查询 `萝莉` 类型的作品，参数值可以填`1`

参数为空时，默认值为`0`，假如参数中包含`0`时，将在随机类型中获取作品

## 响应

| 参数名  | 数据类型 | 说明                              |
| ------- | :------: | --------------------------------- |
| `code`  |  `int`   | 状态码，`0`表示成功，其他均为失败 |
| `error` | `string` | 错误信息                          |
| `data`  | `setu[]` | 色图数组，[详见下文](#setu)       |

### setu

| 字段名        |  数据类型  | 说明                                                       |
| ------------- | :--------: | ---------------------------------------------------------- |
| `pid`         |   `int`    | 作品 pid                                                   |
| `p`           |   `int`    | 作品所在页                                                 |
| `total`       |   `int`    | 作品包含的图片数量                                         |
| `uid`         |   `int`    | 作者 uid                                                   |
| `author`      |  `string`  | 作者名称                                                   |
| `level`       |   `int`    | 社保级别，[详见 level 说明](#level)                        |
| `taste`       |   `int`    | 图片类型，[详见 taste 说明](#taste)，这里`0`表示未分类     |
| `title`       |  `string`  | 作品标题                                                   |
| `description` |  `string`  | 作品描述                                                   |
| `r18`         | `boolean`  | 是否包含 R-18 标签                                         |
| `gif`         | `boolean`  | 是否动图，即包含动图标签                                   |
| `original`    | `boolean`  | 是否原图，pixiv 作品中返回的一个标识                       |
| `aiType`      |   `int`    | 是否是 AI 作品，0 未知（旧画作或字段未更新），1 不是，2 是 |
| `width`       |   `int`    | 原图宽度 px                                                |
| `height`      |   `int`    | 原图高度 px                                                |
| `ext`         |  `string`  | 图片扩展名                                                 |
| `uploadDate`  |   `int`    | 作品上传日期；时间戳，单位为毫秒                           |
| `urls`        |  `object`  | 作品所在页的4种尺寸的图片地址                              |
| `fullUrls`    | `object[]` | 作品所有页的4种尺寸的图片地址                              |
| `tags`        | `string[]` | 作品标签，包含标签的中文翻译（有的话）                     |
| `extags`      | `string[]` | 扩展标签，指本人额外添加的标签（如果有空添加的话）         |

## 示例
<details>
<summary>点击查看示例</summary>

```http
  Get https://lolisuki.cn/api/setu/v1?level=2&tag=拉菲
```

```json
  {
  "code": 0,
  "error": "",
  "data": [
      {
          "pid": 71645447,
          "p": 0,
          "total": 1,
          "uid": 2353373,
          "author": "2drr/ディル@FANBOX",
          "level": 2,
          "taste": 1,
          "title": "ラフィー",
          "description": "",
          "r18": false,
          "gif": false,
          "original": false,
          "width": 1477,
          "height": 1200,
          "ext": "png",
          "uploadDate": 1542151017000,
          "urls": {
              "thumb": "https://i.pixiv.re/c/128x128/img-master/img/2018/11/14/00/16/57/71645447_p0_square1200.jpg",
              "small": "https://i.pixiv.re/c/540x540_70/img-master/img/2018/11/14/00/16/57/71645447_p0_master1200.jpg",
              "regular": "https://i.pixiv.re/img-master/img/2018/11/14/00/16/57/71645447_p0_master1200.jpg",
              "original": "https://i.pixiv.re/img-original/img/2018/11/14/00/16/57/71645447_p0.png"
          },
          "fullUrls": [
                {
                  "thumb": "https://i.pixiv.re/c/128x128/img-master/img/2018/11/14/00/16/57/71645447_p0_square1200.jpg",
                  "small": "https://i.pixiv.re/c/540x540_70/img-master/img/2018/11/14/00/16/57/71645447_p0_master1200.jpg",
                  "regular": "https://i.pixiv.re/img-master/img/2018/11/14/00/16/57/71645447_p0_master1200.jpg",
                  "original": "https://i.pixiv.re/img-original/img/2018/11/14/00/16/57/71645447_p0.png"
                }
            ],
          "tags": [
              "ラフィー",
              "拉菲",
              "Laffey",
              "アズールレーン",
              "碧蓝航线",
              "碧藍航線",
              "Azur Lane",
              "벽람항로",
              "ロリ",
              "萝莉",
              "蘿莉",
              "loli",
              "로리",
              "ラフィー(アズールレーン)",
              "拉菲（碧蓝航线）",
              "拉菲(碧藍航線)",
              "Laffey (Azur Lane)",
              "腋",
              "腋下",
              "armpits",
              "겨드랑이",
              "横臥",
              "侧卧",
              "側躺",
              "lying on one side",
              "アズールレーン5000users入り",
              "碧蓝航线5000收藏",
              "Azur Lane 5000+ bookmarks",
              "はいてない",
              "真空",
              "沒穿內褲",
              "bottomless",
              "노팬티",
              "尻",
              "屁股",
              "ass",
              "엉덩이"
          ],
          "extags": []
      }
  ]
}
```
### p0的效果图
![markdown picture](./img/71645447_p0_master1200.jpg)
</details>

## 已有数据
<div id="total" style="max-width:600px">
  <p>
    <p style="font-size:20px;color:#FFB980;font-weight:bold;">作品数：{{resultData.setuCount}}</p>
    <p style="font-size:20px;color:#FFB980;font-weight:bold;">画师数：{{resultData.userCount}}</p>
  </p>
  <p>
    <ve-ring :data="totalData" :extend="totalExtend" :colors="totalColor" />
  </p>
</div>

<div id="levelRing" style="max-width:600px">
  <ve-ring :data="levelData" :extend="levelExtend" :colors="levelColor" />
</div>

<div id="requestLine" style="max-width:600px">
  <ve-line :data="requestData" :extend="requestExtend" />
</div>

<script>
  let apiUri="https://lolisuki.cn";

  axios.get(`${apiUri}/api/info/v1/TotalCount`).then(function(resultData){
    new Vue({
      el: '#total',
      data: function () {
        let rdata=resultData.data.data;
        let rows=[
          {
            'Type':'R-18',
            '数量':rdata.r18Count
          },
          {
            'Type':'Safe',
            '数量':rdata.setuCount-rdata.r18Count
          }
        ];
        let r18Perce=((rdata.r18Count/rdata.setuCount)*100).toFixed(2)
        return {
          resultData:rdata,
          totalData: {
            columns: ['Type', '数量'],
            rows: rows
          },
          totalExtend: {
            title: {
              text: `R18(${r18Perce}%)`,
              left: 'center',
              top: 'center'
            }
          },
          totalColor: ['#5AB1EF','#19D4AE']
        }
      },
      components: { VeRing }
    })
  });

  axios.get(`${apiUri}/api/info/v1/LevelCount`).then(function(resultData){
    new Vue({
      el: '#levelRing',
      data: function () {
        let rows=[];
        resultData.data.data.forEach((item,index)=>{
          rows.push({'Level':`Level${item.level}(${item.count})`,'数量':item.count})
        });
        return {
          levelData: {
            columns: ['Level', '数量'],
            rows: rows
          },
          levelExtend: {
            title: {
              text: 'Level占比',
              left: 'center',
              top: 'center'
            }
          },
          levelColor: ['#19D4AE','#5AB1EF','#FFF68F','#FFB980','#FA6E86','#DC143C','#8B0000']
        }
      },
      components: { VeRing }
    })
  });

  axios.get(`${apiUri}/api/info/v1/RequestCount`).then(function(resultData){
    new Vue({
      el: '#requestLine',
      data: function () {
        let rows=[];
        resultData.data.data.forEach((item,index)=>{
          rows.push({'日期':item.date,'请求量':item.count})
        });
        return {
          requestData: {
            columns: ['日期', '请求量'],
            rows: rows
          },
          requestExtend: {
            title: {
              text: '近7日请求量',
              left: 'center',
              top: 'center'
            }
          }
        }
      },
      components: { VeLine }
    })
  });


</script>
