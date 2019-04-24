---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# 批次号前端接口需求

前端整理的开发接口需求，item 指的是数组里的每一项，最好返回批次号及生产日期，详见http://192.168.2.23/member-service/board/issues/1059

(This API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.)

# 商品列表

## 热销商品

> Interface should return JSON structured like this:

```javascript
"result": [
        {
          **"添加字段":"批次号字段",**
          itemId: "3467",
          itemName: "冰露纯悦包装饮用水（纸箱）",
          itemImageAddress1:
            "http://pro-img-jihuiduo.oss-cn-beijing.aliyuncs.com/sku_image/3467.jpg",
          price: 19,
          itemCategoryCode: "1204001",
          itemSpecification: "550ml*24",
          promotionTypes: ["2"],
          saleUnit: "箱(24个)",
          // "putShelvesFlg": '@boolean',
          putShelvesFlg,
          inventoryCount: "@pick([0, 1])"
        }
      ]
```

> item 添加字段

如果是非临期品，就为空，如果是临期品就返回批次号，返回时间最早的批次号

`item添加字段`

### HTTP Request

`GET mall/items/hot?locationId=128&start=0&limit=20&merchantId=27602`

<aside class="notice">
<!-- You must replace <code>meowmeowmeow</code> with your personal API key. -->
因为首页可以直接加入商品到进货单，所以需要有批次号字段
</aside>
## 商品列表

> 在该页面购买时会列出所有批次信息

> Interface should return JSON structured like this:

```json
{
  "message": "OK",
  "status": 200,
  "result": [
    {
      **"添加字段": "批次号字段",**
      "putShelvesFlg": true,
      "inventoryCount": 20,
      "itemBrandId": "90575",
      "seriesCode": "68235",
      "itemId": "3467",
      "itemName": "冰露纯悦包装饮用水（纸箱）",
      "itemSpecification": "550ml*24",
      "itemBrand": "可口可乐",
      "saleUnit": "箱(24个)",
      "stockUnit": "个",
      "saleSku": "6928804010015",
      "stockSku": "6928804010015",
      "itemImageAddress1": "http://pro-img-jihuiduo.oss-cn-beijing.aliyuncs.com/sku_image/3467.jpg",
      "itemImageAddress2": "",
      "itemImageAddress3": "",
      "itemImageAddress4": "",
      "itemImageAddress5": "",
      "applayScope": "2",
      "salesDescription": "",
      "itemLocationCollection": "12,130,140,145,146,18,20,40,44,46,49,54,55,56,57,72,73,75,76,79,86,87,119,122,127,128,129,131,132,133,134,135,136,137,138,139,143,144",
      "itemCategoryCode": "1204001",
      "itemOrigin": "",
      "itemExpirationDays": "",
      "putShelvesDate": "2018/10/29 14:31:05",
      "price": 118.2,
      "itemCategoryName": " 可口可乐",
      "priceLevel": "47",
      "promotionTypes": null,
      "saleUnitExchange": 24,
      "purchaseType": "",
      "itemPackage": "瓶",
      "length": "",
      "width": "",
      "height": "",
      "weight": "",
      "isConsignedGood": "N"
    }
  ]
}
```

商品详情页面购买时会列出所有批次信息

### HTTP Request

`GET mall/items?locationId=140&categoryCd=&itemIds=3467&merchantId=30263`

<aside class="notice">
需要返回该商品所有批次信息，包括批次号和生产日期
</aside>

# 进货单相关

## 加入进货单

> 请求参数的每个商品都添加批次号字段，如为非临期品则为空. Requests with JSON structured like this:

```json
{
  "locationId": 128,
  "merchantId": "27602",
  "count": 1,
  "groupId": 1000,
  "promotions": [{ "promotionId": 11665 }],
  "addItemList": [
    {
      **"添加字段": "批次号字段",**
      "putShelvesFlg": true,
      "inventoryCount": 1,
      "seriesCode": "0253",
      "itemBrandId": "wgfurvsdeo",
      "itemId": 1000,
      "itemName": "再报是转照八克他",
      "itemSpecification": "420ml*12",
      "itemBrand": "可口可乐",
      "saleUnit": "盒(7个)",
      "stockUnit": "个",
      "saleSku": "6956416205147",
      "stockSku": "6956416205147",
      "itemImageAddress1": "http://dummyimage.com/468x60",
      "itemImageAddress2": "",
      "itemImageAddress3": "",
      "itemImageAddress4": "",
      "itemImageAddress5": "",
      "applayScope": "2",
      "salesDescription": "",
      "itemLocationCollection": "12,130,145,146,140,44,18,20,40,46,49,128,54,55,56,57,72,73,75,76,129,79,131,132,133,134,135,136,86,87,119,122,137,138,127,139,143,144",
      "itemCategoryCode": "1205014",
      "itemOrigin": "",
      "itemExpirationDays": "",
      "putShelvesDate": "2018/11/01 14:08:22",
      "price": "58.00",
      "quantity": 1,
      "addTime": "2018-12-18T01:51:22.270+0000",
      "categoryCode": "1205014"
    }
  ],
  "updateAddTime": false
}
```

加入进货单时传入批次号字段

### HTTP Request

`POST /mall/cart/add/{merchantId}/{locationId}`

<aside class="notice">请求参数的每个商品都添加批次号字段，如为非临期品则为空</aside>

### URL Parameters

| Parameter   | Type  |
| ----------- | ----- |
| addItemList | array |

## 进货单列表

> 按不同批次号展示所有购买的商品. The interface should return JSON structured like this:

```javascript
{
  [`items|${len}`]: [
          {
            **"添加字段": "批次号字段",**
            "putShelvesFlg": false,
            "putShelvesFlg": true,
            inventoryCount,
            seriesCode: '@string("number",4)',
            itemBrandId: '@string("lower",10)',
            itemId: '@increment(1000)',
            "itemName": "@ctitle(5,10)",
            "itemSpecification": "420ml*12",
            "itemBrand": "可口可乐",
            "saleUnit": "@pick(['箱','盒'])(@integer(5,10)个)",
            "stockUnit": "个",
            "saleSku": "6956416205147",
            "stockSku": "6956416205147",
            "itemImageAddress1": "@image",
            // "itemImageAddress1": 'https://stg-statics.jihuiduo.cn/jhb_images/%E6%83%A0%E7%99%BE%E7%9C%9F%E6%B4%97%E8%A1%A3%E6%B6%B21.jpg',
            "itemImageAddress2": "",
            "itemImageAddress3": "",
            "itemImageAddress4": "",
            "itemImageAddress5": "",
            "applayScope": "2",
            "salesDescription": "",
            "itemLocationCollection": "12,130,145,146,140,44,18,20,40,46,49,128,54,55,56,57,72,73,75,76,129,79,131,132,133,134,135,136,86,87,119,122,137,138,127,139,143,144",
            "itemCategoryCode": "1205014",
            "itemOrigin": "",
            "itemExpirationDays": "",
            "putShelvesDate": "2018/11/01 14:08:22",
            "price": '@integer(10,100)',
            "quantity": '@integer(1,3)',
            "addTime": "2018-12-18T01:51:22.270+0000"
          }
        ]
}
```

返回的进货单商品按照加入时传入的批次号返回，若加入多个批次号不合并

### HTTP Request

`GET /mall/cart/{merchantId}/{locationId}?start={start}&limit={limit}`

<aside class="notice">按不同批次号展示所有购买的商品</aside>
