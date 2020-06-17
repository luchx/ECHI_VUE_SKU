# 使用 Vue 实现购物车 SKU 选择

> 做过商城项目的对于 SKU 来说应该不会陌生，那么前端如何实现 SKU 的动态匹配呢？相信对于第一次做商城项目的小伙伴们来说应该是很费解吧，因为涉及到商品库存、价格、和规格属性等问题。下面就由我来为大家展示下如何处理 SKU 及实现购物车 SKU 选择。

## 一、数据结构

首先我们先看下后端返回的数据结构（这里结构可能会有点差异，但实现原理大致相同）：

```json
{
  "material_id": 70,
  "trade_name": "Apple iPhone 11 (A2223) 128GB 黑色 移动联通电信4G手机 双卡双待",
  "main_img": DEFAULT_PNG,
  "sku_list": [
    {
      "sku_id": 101,
      "material_id": 70,
      "sku_price": 179,
      "main_img": SKU_1_PNG,
      "sku_prop": [
        {
          "attribute_id": 58,
          "attribute_name": "颜色",
          "attribute_value_id": 143,
          "attribute_value": "白色"
        },
        {
          "attribute_id": 108,
          "attribute_name": "内存",
          "attribute_value_id": 243,
          "attribute_value": "32G"
        },
        {
          "attribute_id": 208,
          "attribute_name": "上市年份",
          "attribute_value_id": 343,
          "attribute_value": "2019"
        }
      ],
      "stock": 190
    },
    {
      "sku_id": 102,
      "material_id": 70,
      "sku_price": 179,
      "main_img": SKU_1_PNG,
      "sku_prop": [
        {
          "attribute_id": 58,
          "attribute_name": "颜色",
          "attribute_value_id": 143,
          "attribute_value": "白色"
        },
        {
          "attribute_id": 108,
          "attribute_name": "内存",
          "attribute_value_id": 243,
          "attribute_value": "32G"
        },
        {
          "attribute_id": 208,
          "attribute_name": "上市年份",
          "attribute_value_id": 344,
          "attribute_value": "2020"
        }
      ],
      "stock": 190
    },
    {
      "sku_id": 103,
      "material_id": 70,
      "sku_price": 179,
      "main_img": SKU_1_PNG,
      "sku_prop": [
        {
          "attribute_id": 58,
          "attribute_name": "颜色",
          "attribute_value_id": 143,
          "attribute_value": "白色"
        },
        {
          "attribute_id": 108,
          "attribute_name": "内存",
          "attribute_value_id": 244,
          "attribute_value": "64G"
        },
        {
          "attribute_id": 208,
          "attribute_name": "上市年份",
          "attribute_value_id": 343,
          "attribute_value": "2019"
        }
      ],
      "stock": 190
    },
    {
      "sku_id": 106,
      "material_id": 70,
      "sku_price": 183,
      "main_img": SKU_2_PNG,
      "sku_prop": [
        {
          "attribute_id": 58,
          "attribute_name": "颜色",
          "attribute_value_id": 144,
          "attribute_value": "红色"
        },
        {
          "attribute_id": 108,
          "attribute_name": "内存",
          "attribute_value_id": 243,
          "attribute_value": "32G"
        },
        {
          "attribute_id": 208,
          "attribute_name": "上市年份",
          "attribute_value_id": 343,
          "attribute_value": "2019"
        }
      ],
      "stock": 223
    },
    {
      "sku_id": 109,
      "material_id": 70,
      "sku_price": 183,
      "main_img": SKU_2_PNG,
      "sku_prop": [
        {
          "attribute_id": 58,
          "attribute_name": "颜色",
          "attribute_value_id": 144,
          "attribute_value": "红色"
        },
        {
          "attribute_id": 108,
          "attribute_name": "内存",
          "attribute_value_id": 244,
          "attribute_value": "64G"
        },
        {
          "attribute_id": 208,
          "attribute_name": "上市年份",
          "attribute_value_id": 344,
          "attribute_value": "2020"
        }
      ],
      "stock": 223
    },
    {
      "sku_id": 107,
      "material_id": 70,
      "sku_price": 200,
      "main_img": SKU_2_PNG,
      "sku_prop": [
        {
          "attribute_id": 58,
          "attribute_name": "颜色",
          "attribute_value_id": 145,
          "attribute_value": "蓝色"
        },
        {
          "attribute_id": 108,
          "attribute_name": "内存",
          "attribute_value_id": 243,
          "attribute_value": "32G"
        },
        {
          "attribute_id": 208,
          "attribute_name": "上市年份",
          "attribute_value_id": 343,
          "attribute_value": "2019"
        }
      ],
      "stock": 223
    },
    {
      "sku_id": 108,
      "material_id": 70,
      "sku_price": 200,
      "main_img": SKU_2_PNG,
      "sku_prop": [
        {
          "attribute_id": 58,
          "attribute_name": "颜色",
          "attribute_value_id": 145,
          "attribute_value": "蓝色"
        },
        {
          "attribute_id": 108,
          "attribute_name": "内存",
          "attribute_value_id": 244,
          "attribute_value": "64G"
        },
        {
          "attribute_id": 208,
          "attribute_name": "上市年份",
          "attribute_value_id": 343,
          "attribute_value": "2019"
        }
      ],
      "stock": 223
    },
    {
      "sku_id": 109,
      "material_id": 70,
      "sku_price": 200,
      "main_img": SKU_2_PNG,
      "sku_prop": [
        {
          "attribute_id": 58,
          "attribute_name": "颜色",
          "attribute_value_id": 145,
          "attribute_value": "蓝色"
        },
        {
          "attribute_id": 108,
          "attribute_name": "内存",
          "attribute_value_id": 244,
          "attribute_value": "64G"
        },
        {
          "attribute_id": 208,
          "attribute_name": "上市年份",
          "attribute_value_id": 344,
          "attribute_value": "2020"
        }
      ],
      "stock": 223
    }
  ]
}
```

## 二、先前准备

> 当我们拿到这样一份数据我们需要先进行数据格式化，并输出形成以下数据：

```javascript
// 在 computed 中对数据进行输出，并得到三分数据：
// attrGroupList
// existSkuIdKey
// attrSameKey

skuListMap() {
  const { sku_list } = this.dataSource;
  // 属性组数据
  const attributeGroupList = {};
  // 属性 sku_id 组
  const existSkuIdKey = {};
  // 同属性组
  const attrSameKey = {};
  (sku_list || []).forEach(list => {
    const { sku_prop } = list;
    // 获取属性组的 attribute_value_id，排序后用 | 分割
    const attrsIdKeys = (sku_prop || [])
      .map(item => item.attribute_value_id)
      .sort((a, b) => a - b)
      .join("|");

    existSkuIdKey[attrsIdKeys] = list;

    (sku_prop || []).forEach(prop => {
      const valueInfo = {
        attribute_value_id: prop.attribute_value_id,
        attribute_value: prop.attribute_value
      };
      const hasAttrId = attributeGroupList[prop.attribute_id];
      // 属性组里不存在则新增
      if (hasAttrId === undefined) {
        attributeGroupList[prop.attribute_id] = {
          ...prop,
          valueList: []
        };
        attrSameKey[prop.attribute_id] = [];
      }
      // 属性列表
      const existList = attributeGroupList[prop.attribute_id].valueList;
      // 判断 attribute_value_id 是否已存在属性组, 防止同一 attribute_value_id 多次添加
      const hasExistAttrId = existList.some(
        item => item.attribute_value_id === prop.attribute_value_id
      );
      if (!hasExistAttrId) {
        Array.prototype.push.call(
          attributeGroupList[prop.attribute_id].valueList,
          valueInfo
        );
        Array.prototype.push.call(
          attrSameKey[prop.attribute_id],
          prop.attribute_value_id
        );
      }
    });
  });

  const attrGroupList = Object.values(attributeGroupList);

  return {
    attrGroupList,
    existSkuIdKey,
    attrSameKey
  };
}
```

### 1. 定义一个数组存储规格列表展示的数据：attrGroupList

> 根据后端返回的数据进行格式化，并输出以下格式，用于渲染规格列表

```javascript
[
  {
    attribute_id: 58,
    attribute_name: "颜色",
    valueList: [
      {
        attribute_value_id: 143,
        attribute_value: "白色",
      },
      {
        attribute_value_id: 144,
        attribute_value: "红色",
      },
      {
        attribute_value_id: 145,
        attribute_value: "蓝色",
      },
    ],
  },
  {
    attribute_id: 108,
    attribute_name: "内存",
    valueList: [
      {
        attribute_value_id: 243,
        attribute_value: "32G",
      },
      {
        attribute_value_id: 244,
        attribute_value: "64G",
      },
    ],
  },
  {
    attribute_id: 208,
    attribute_name: "上市年份",
    valueList: [
      {
        attribute_value_id: 343,
        attribute_value: "2019",
      },
      {
        attribute_value_id: 344,
        attribute_value: "2020",
      },
    ],
  },
];
```

### 2. 定义一个对象存储同一规格组的数据：attrSameKey

```javascript
{
  "58": [
    143,
    144,
    145
  ],
  "108": [
    243,
    244
  ],
  "208": [
    343,
    344
  ]
}
```

### 3. 定义一个对象用于判断是否存在 sku_id 匹配的数据：existSkuIdKey

```javascript
{
  "143|243|343": {
    "sku_id": 101,
    "material_id": 70,
    "sku_price": 179,
    "main_img": "",
    "stock": 190
  },
  "143|243|344": {
    "sku_id": 102,
    "material_id": 70,
    "sku_price": 179,
    "main_img": "",
    "stock": 190
  },
  "143|244|343": {
    "sku_id": 103,
    "material_id": 70,
    "sku_price": 179,
    "main_img": "",
    "stock": 190
  },
  "144|243|343": {
    "sku_id": 106,
    "material_id": 70,
    "sku_price": 183,
    "main_img": "",
    "stock": 223
  },
  "144|244|344": {
    "sku_id": 109,
    "material_id": 70,
    "sku_price": 183,
    "main_img": "",
    "stock": 223
  },
  "145|243|343": {
    "sku_id": 107,
    "material_id": 70,
    "sku_price": 200,
    "main_img": "",
    "stock": 223
  },
  "145|244|343": {
    "sku_id": 108,
    "material_id": 70,
    "sku_price": 200,
    "main_img": "",
    "stock": 223
  },
  "145|244|344": {
    "sku_id": 109,
    "material_id": 70,
    "sku_price": 200,
    "main_img": "",
    "stock": 223
  }
}
```

## 三、SKU 禁用判断

> 禁用规则说明：
>
>1. 存在选项且小于可选项，不满足匹配规则
>2. 存在同组数据，且选项未选完
>3. 不在存在 sku 中

```javascript
// 在 methods 中定义一个方法，接受一个 attribute_value_id 进行判断，并返回 Boolean

disabledKey(attribute_value_id) {
  const attrSameKey = Object.values(this.skuListMap.attrSameKey);
  const selectKeys = Object.values(this.activeKey);
  const validLen = this.skuListMap.attrGroupList.length;
  // 存在选项且小于可选项，不满足匹配规则，直接返回 false
  if (selectKeys.length < validLen - 1) {
    return false;
  }
  // 获取选项中属于同组的数据
  const filterSameKey = attrSameKey.filter(arr => {
    return selectKeys.find(key => arr.includes(key));
  });

  // 获取同一属性组数据
  const sameGroupKey = filterSameKey.filter(arr =>
    arr.includes(attribute_value_id)
  );

  // 存在同组数据，且选项未选完
  if (selectKeys.length !== validLen && sameGroupKey.length) {
    return false;
  }

  // 取出 attribute_value_id
  const { existSkuIdKey } = this.skuListMap;
  // 去除与当前 attribute_value_id 匹配的同组数据
  const aloneKeys = selectKeys.filter(key => {
    return !sameGroupKey.some(sameKeys => sameKeys.includes(key));
  });
  const selectSkuGroup = [...aloneKeys, attribute_value_id];
  const skuKeyGroup = selectSkuGroup.sort((a, b) => a - b).join("|");
  // 判断是否存在 SKU 组中
  const hasInSku = Reflect.has(existSkuIdKey, skuKeyGroup);

  // 不在存在 sku 中
  return !hasInSku;
}
```

## 四、判断购物车选项状态，并返回选中结果

```javascript
validateCartStatus() {
  const { material_id, sku_list } = this.dataSource;
  const cartNumber = this.cartNumber;
  if (typeof cartNumber !== "number" || cartNumber < 1) {
    return {
      status: false,
      params: {}
    };
  }

  if (!sku_list || sku_list.length === 0) {
    // TODO: 未定义场景（异常）
    return {
      status: false,
      params: {}
    };
  }

  const firstSku = sku_list[0];
  // 如果只存在一条 sku 记录，则默认选取
  if (sku_list.length === 1 && (firstSku.sku_prop || []).length === 0) {
    return {
      status: true,
      item: firstSku,
      params: {
        material_id,
        sku_id: firstSku.sku_id,
        quantity: cartNumber
      }
    };
  }

  // 当前选中的 sku 属性
  const activeKey = this.activeKey;
  const { existSkuIdKey } = this.skuListMap;
  // 取出当前选中的 sku 组
  const selectSkuGroup = Object.values(activeKey);

  const skuKeyGroup = selectSkuGroup.sort((a, b) => a - b).join("|");
  // 获取当前 sku_id 对应的属性组
  const findSkuItem = Reflect.get(existSkuIdKey, skuKeyGroup);

  const selectActiveKeys = Object.keys(activeKey);
  if (!findSkuItem || selectActiveKeys.length === 0) {
    return {
      status: false,
      params: {}
    };
  }

  return {
    status: true,
    item: findSkuItem,
    params: {
      material_id,
      sku_id: findSkuItem.sku_id,
      quantity: cartNumber
    }
  };
}
```

## 五、代码实现

> 具体代码实现请前往 [github 查看](https://github.com/luchx/VUE_CART_SKU)，或点击下方图标查看

[![codeSandbox.svg](https://cdn.nlark.com/yuque/0/2020/svg/373912/1591508056037-fc5b8a9d-5c64-4f24-ad74-1d6c8ffeae8b.svg#align=left&display=inline&height=42&margin=%5Bobject%20Object%5D&originHeight=42&originWidth=201&size=0&status=done&style=none&width=201#align=left&display=inline&height=42&margin=%5Bobject%20Object%5D&originHeight=42&originWidth=201&status=done&style=none&width=201#align=left&display=inline&height=42&margin=%5Bobject%20Object%5D&originHeight=42&originWidth=201&status=done&style=none&width=201)](https://codesandbox.io/s/vue-cart-sku-299nt?fontsize=14&hidenavigation=1&theme=dark)

## 六、最终效果

![image.png](https://cdn.nlark.com/yuque/0/2020/png/373912/1591506721696-3abb7a89-f569-445a-a84d-889e432a06d8.png#align=left&display=inline&height=696&margin=%5Bobject%20Object%5D&name=image.png&originHeight=696&originWidth=401&size=52490&status=done&style=none&width=401#align=left&display=inline&height=696&margin=%5Bobject%20Object%5D&originHeight=696&originWidth=401&status=done&style=none&width=401#align=left&display=inline&height=696&margin=%5Bobject%20Object%5D&originHeight=696&originWidth=401&status=done&style=none&width=401)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/373912/1591507235477-0333b109-9b1c-4f1c-9f78-96d69552ee1b.png#align=left&display=inline&height=688&margin=%5Bobject%20Object%5D&name=image.png&originHeight=688&originWidth=395&size=62118&status=done&style=none&width=395#align=left&display=inline&height=688&margin=%5Bobject%20Object%5D&originHeight=688&originWidth=395&status=done&style=none&width=395#align=left&display=inline&height=688&margin=%5Bobject%20Object%5D&originHeight=688&originWidth=395&status=done&style=none&width=395)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/373912/1591507849846-bfa08971-6213-4fd5-b865-abf37404756a.png#align=left&display=inline&height=688&margin=%5Bobject%20Object%5D&name=image.png&originHeight=688&originWidth=399&size=37091&status=done&style=none&width=399#align=left&display=inline&height=688&margin=%5Bobject%20Object%5D&originHeight=688&originWidth=399&status=done&style=none&width=399#align=left&display=inline&height=688&margin=%5Bobject%20Object%5D&originHeight=688&originWidth=399&status=done&style=none&width=399)
