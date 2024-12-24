# BUAA Annual Eat

> 项目的 IDEA 来源于 [THU-Annual-Eat](https://github.com/leverimmy/THU-Annual-Eat)。

## 项目简介

## 使用方法

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
