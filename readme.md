# BUAA Annual Eat

> 本项目的 IDEA 来源于 [THU-Annual-Eat](https://github.com/leverimmy/THU-Annual-Eat)。

## 项目简介

本项目用于统计百航学生的校园卡消费情况并可视化，全程在浏览器中操作，无需配置环境和编写配置文件😀。

按地点统计，柱状图示例：

![](/demo/img-1.png)

按地点统计，饼状图示例：

![](/demo/img-2.png)

按月份统计，柱状图示例：

![](/demo/img-3.png)

按月份统计，饼状图示例：

![](/demo/img-4.png)

按时段统计，柱状图示例：

![](/demo/img-5.png)

按时段统计，饼状图示例：

![](/demo/img-6.png)

## 使用方法

Step 1，访问[航财通·校园付](https://cc-pay.cn/login)网站并登录，推荐采用统一认证登录：

![](/step/img-1.jpg)

Step 2，登录[航财通·校园付](https://cc-pay.cn/login)网站后，点击页面右上角的首页按钮：

![](/step/img-2.jpg)

Step 3，在首页中，点击屏幕左侧“校园卡余额”下方的“流水”按钮：

![](/step/img-3.jpg)

Step 4，进入流水信息查询页，实际 URL 中的 stuNo 是你的学工号，下图进行了隐藏：

![](/step/img-4.jpg)

Step 5，在流水信息查询页点击 <kbd>F12</kbd> 进入开发者工具，粘贴附录中的代码并回车执行：

![](/step/img-5.jpg)

Step 6，稍等几秒，控制台中会输出一大段文本，直接点击结尾的复制按钮：

![](/step/img-6.jpg)

Step 7，访问[可视化前端](https://chenrt-ggx.github.io/BUAA-Annual-Eat)页面，粘贴上一步复制的内容，点击解析列表即可。在此基础上，你可以按需要隐藏部分消费地点，和自定义图表标题，Have Fun！😋😋😋

![](/step/img-7.jpg)

## 附录

```js
const query = async () => {

  let base = '/api/campus_card/trades'; // target api endpoint
  let minTsCreation = new Date().getFullYear() - (new Date().getMonth() < 6 ? 1 : 0);
  let maxTsCreation = new Date().getFullYear() - (new Date().getMonth() < 6 ? 1 : 0);
  let stuNo = new URLSearchParams(location.search).get('stuNo');

  base += `?t=${+new Date()}&pageSize=${100000}&pageNum=1`;
  base += `&minTsCreation=${minTsCreation + '-01-01T00:00:00.000Z'}`;
  base += `&maxTsCreation=${maxTsCreation + '-12-31T23:59:59.999Z'}`;
  base += `&tradeType=all&stuNo=${stuNo}`; // authenticated

  let json = await fetch(base, { cache: 'no-store' }).then((response) => response.json());
  /*
  [
    { "date": str, "amount": int, "merName": str, "balance": int, "txName": str },
    { "date": str, "amount": int, "merName": str, "balance": int, "txName": str },
    { "date": str, "amount": int, "merName": str, "balance": int, "txName": str }
  ]
  */
  return json.data.data.filter((node) => node.amount < 0);
};

console.log(JSON.stringify(await query()));
```

查看入学以来的所有消费

```js
classOf = "YYYY" // 入学年份
const query = async (classOf) => {

  let base = '/api/campus_card/trades'; // target api endpoint
  let minTsCreation = new Date().getFullYear() - (new Date().getMonth() < 6 ? 1 : 0);
  let maxTsCreation = new Date().getFullYear() - (new Date().getMonth() < 6 ? 1 : 0);
  let stuNo = new URLSearchParams(location.search).get('stuNo');

  base += `?t=${+new Date()}&pageSize=${100000}&pageNum=1`;
  base += `&minTsCreation=${ classOf + '-01-01T00:00:00.000Z'}`;
  base += `&maxTsCreation=${maxTsCreation + '-12-31T23:59:59.999Z'}`;
  base += `&tradeType=all&stuNo=${stuNo}`; // authenticated

  let json = await fetch(base, { cache: 'no-store' }).then((response) => response.json());
  /*
  [
    { "date": str, "amount": int, "merName": str, "balance": int, "txName": str },
    { "date": str, "amount": int, "merName": str, "balance": int, "txName": str },
    { "date": str, "amount": int, "merName": str, "balance": int, "txName": str }
  ]
  */
  return json.data.data.filter((node) => node.amount < 0);
};

console.log(JSON.stringify(await query(classOf)));
```
