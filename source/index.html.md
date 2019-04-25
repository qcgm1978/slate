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
      "itemName": "冰露纯悦包装饮用水（纸箱）"
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
      "itemImageAddress1": "http://dummyimage.com/468x60"
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
            "itemBrand": "可口可乐"
          }
        ]
}
```

返回的进货单商品按照加入时传入的批次号返回，若加入多个批次号不合并

### HTTP Request

`GET /mall/cart/{merchantId}/{locationId}?start={start}&limit={limit}`

<aside class="notice">按不同批次号展示所有购买的商品</aside>

# 订单相关

## 订单列表

> 在该页面再次购买时须传递批次信息

> Interface should return JSON structured like this:

```javascript
{
  "message": "OK",
  "status": 200,
  "result": [
    {
      orderItems: [
        {
                promotionId: null,
                discountAmount: 0,
                "items|10": [
                  {
                    **"添加字段": "批次号字段",**
                    itemId: "3351",
                    itemSku: null,
                    itemName: "一级大豆油",
                    quantity: 1
                  }
                ]
        }
    }
  ]
}
```

### HTTP Request

`POST mall/order/list`

<aside class="notice">
订单列表接口需要返回各订单所含商品相应批次信息，包括批次号和生产日期
</aside>

## 订单详情

> 在该页面再次购买时须传递批次信息

> Interface should return JSON structured like this:

```json
{
  "message": "OK",
  "status": 200,
  "result": {
    "orderItems": [
      {
        "groupId": "dd2d1c9edd8511e8b0684f66c7dc16ed",
        "promotionId": null,
        "discountAmount": 0,
        "items": [
          {
            **"添加字段": "批次号字段",**
            "itemId": "5911",
            "itemSku": null,
            "itemName": "规内二书没离导却标认定照图层极",
            "quantity": 4,
            "unitPrice": 144,
            "saleUnit": "个",
            "locationId": "140",
            "itemIcon": "https://pro-img-jihuiduo.oss-cn-beijing.aliyuncs.com/sku_image/5911.png",
            "itemSpecification": "750g*12*16",
            "returnQuantit": 4,
            "categoryId": "2301001",
            "promotionId": null,
            "gift": false
          }
        ]
      }
    ]
  }
}
```

### HTTP Request

`GET mall/order/81763/181029143259643356`

<aside class="notice">
订单详情接口需要返回所含商品相应批次信息，包括批次号和生产日期
</aside>
