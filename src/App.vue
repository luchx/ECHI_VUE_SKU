<template>
  <div id="app" class="cart-box">
    <div class="cart-main">
      <div class="cart-card">
        <div class="cart-row">
          <img class="cart-img" :src="activeImg" />
          <div class="cart-info">
            <div class="cart-name">{{ dataSource.trade_name }}</div>
            <div class="cart-money">¥ {{ price }}</div>
            <div class="cart-stock" v-show="stock">库存 {{ stock }} 件</div>
          </div>
        </div>
      </div>
      <div class="cart-card">
        <div class="cart-card-container" v-for="sku in skuListMap.attrGroupList" :key="sku.attribute_id">
          <div class="cart-card-title">{{sku.attribute_name}}</div>
          <div class="cart-card-box">
            <template v-for="tag in sku.valueList">
              <span
                :class="['cart-card-item', 
                    { 'active': activeKey[sku.attribute_id] === tag.attribute_value_id },
                    { 'disabled': disabledKey(tag.attribute_value_id) },
                  ]"
                :key="tag.attribute_value_id"
                @click="handleClick(sku, tag)"
              >{{tag.attribute_value}}</span>
            </template>
          </div>
        </div>
      </div>
    </div>
    <div
      :class="['cart-footer', { 'disabled': !validateCartStatus.status}]"
      @click="handleAddToCart"
    >加入购物车</div>
  </div>
</template>

<script>
import DEFAULT_PNG from "@/assets/default.png";
import SKU_1_PNG from "@/assets/1.png";
import SKU_2_PNG from "@/assets/2.png";

export default {
  name: "Cart",
  data() {
    return {
      activeKey: {},
      cartNumber: 1,
      dataSource: {
        material_id: 70,
        trade_name:
          "铝艺大门别墅庭院门铝合金对开门家用乡村折叠门电动院子双开大门 支持定做",
        main_img: DEFAULT_PNG,
        sku_list: [
          {
            sku_id: 101,
            material_id: 70,
            sku_price: 179,
            main_img: SKU_1_PNG,
            sku_prop: [
              {
                attribute_id: 58,
                attribute_name: "颜色",
                attribute_value_id: 143,
                attribute_value: "白色"
              },
              {
                attribute_id: 108,
                attribute_name: "内存",
                attribute_value_id: 243,
                attribute_value: "32G"
              },
              {
                attribute_id: 208,
                attribute_name: "上市年份",
                attribute_value_id: 343,
                attribute_value: "2019"
              }
            ],
            stock: 190
          },
          {
            sku_id: 102,
            material_id: 70,
            sku_price: 179,
            main_img: SKU_1_PNG,
            sku_prop: [
              {
                attribute_id: 58,
                attribute_name: "颜色",
                attribute_value_id: 143,
                attribute_value: "白色"
              },
              {
                attribute_id: 108,
                attribute_name: "内存",
                attribute_value_id: 243,
                attribute_value: "32G"
              },
              {
                attribute_id: 208,
                attribute_name: "上市年份",
                attribute_value_id: 344,
                attribute_value: "2020"
              }
            ],
            stock: 190
          },
          {
            sku_id: 103,
            material_id: 70,
            sku_price: 179,
            main_img: SKU_1_PNG,
            sku_prop: [
              {
                attribute_id: 58,
                attribute_name: "颜色",
                attribute_value_id: 143,
                attribute_value: "白色"
              },
              {
                attribute_id: 108,
                attribute_name: "内存",
                attribute_value_id: 244,
                attribute_value: "64G"
              },
              {
                attribute_id: 208,
                attribute_name: "上市年份",
                attribute_value_id: 343,
                attribute_value: "2019"
              }
            ],
            stock: 190
          },
          {
            sku_id: 106,
            material_id: 70,
            sku_price: 183,
            main_img: SKU_2_PNG,
            sku_prop: [
              {
                attribute_id: 58,
                attribute_name: "颜色",
                attribute_value_id: 144,
                attribute_value: "红色"
              },
              {
                attribute_id: 108,
                attribute_name: "内存",
                attribute_value_id: 243,
                attribute_value: "32G"
              },
              {
                attribute_id: 208,
                attribute_name: "上市年份",
                attribute_value_id: 343,
                attribute_value: "2019"
              }
            ],
            stock: 223
          },
          {
            sku_id: 109,
            material_id: 70,
            sku_price: 183,
            main_img: SKU_2_PNG,
            sku_prop: [
              {
                attribute_id: 58,
                attribute_name: "颜色",
                attribute_value_id: 144,
                attribute_value: "红色"
              },
              {
                attribute_id: 108,
                attribute_name: "内存",
                attribute_value_id: 244,
                attribute_value: "64G"
              },
              {
                attribute_id: 208,
                attribute_name: "上市年份",
                attribute_value_id: 344,
                attribute_value: "2020"
              }
            ],
            stock: 223
          },
          {
            sku_id: 107,
            material_id: 70,
            sku_price: 200,
            main_img: SKU_2_PNG,
            sku_prop: [
              {
                attribute_id: 58,
                attribute_name: "颜色",
                attribute_value_id: 145,
                attribute_value: "蓝色"
              },
              {
                attribute_id: 108,
                attribute_name: "内存",
                attribute_value_id: 243,
                attribute_value: "32G"
              },
              {
                attribute_id: 208,
                attribute_name: "上市年份",
                attribute_value_id: 343,
                attribute_value: "2019"
              }
            ],
            stock: 223
          },
          {
            sku_id: 108,
            material_id: 70,
            sku_price: 200,
            main_img: SKU_2_PNG,
            sku_prop: [
              {
                attribute_id: 58,
                attribute_name: "颜色",
                attribute_value_id: 145,
                attribute_value: "蓝色"
              },
              {
                attribute_id: 108,
                attribute_name: "内存",
                attribute_value_id: 244,
                attribute_value: "64G"
              },
              {
                attribute_id: 208,
                attribute_name: "上市年份",
                attribute_value_id: 343,
                attribute_value: "2019"
              }
            ],
            stock: 223
          },
          {
            sku_id: 109,
            material_id: 70,
            sku_price: 200,
            main_img: SKU_2_PNG,
            sku_prop: [
              {
                attribute_id: 58,
                attribute_name: "颜色",
                attribute_value_id: 145,
                attribute_value: "蓝色"
              },
              {
                attribute_id: 108,
                attribute_name: "内存",
                attribute_value_id: 244,
                attribute_value: "64G"
              },
              {
                attribute_id: 208,
                attribute_name: "上市年份",
                attribute_value_id: 344,
                attribute_value: "2020"
              }
            ],
            stock: 223
          }
        ]
      }
    };
  },
  computed: {
    activeImg() {
      const defaultImg = this.dataSource.main_img;
      const skuImg = this.validateCartStatus.item?.main_img;
      return skuImg || defaultImg;
    },
    price() {
      const sku_price = this.validateCartStatus.item?.sku_price;
      return sku_price || 0;
    },
    stock() {
      const stock = this.validateCartStatus.item?.stock;
      return stock || 0;
    },
    skuListMap(dataSource) {
      const { sku_list } = dataSource;
      // 属性组数据
      const attributeGroupList = {};
      // 属性 sku_id 组
      const existSkuIdKey = {};
      // 同属性组
      const attrSameKey = {};
      (sku_list || []).forEach(list => {
        const { sku_prop } = list;
        // 获取属性组的 attribute_value_id，用 | 分割
        const attrsIdKeys = (sku_prop || [])
          .map(item => item.attribute_value_id)
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
      
      return {
        attrGroupList: Object.values(attributeGroupList),
        existSkuIdKey: existSkuIdKey,
        attrSameKey: attrSameKey
      };
    },
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
      // 取出 attribute_value_id 以 | 分割, 一维数组,
      const skuAttrListKey = Object.keys(existSkuIdKey);
      // 取出当前选中的 sku 组，一维数组
      const values = Object.values(activeKey);
      // 判断是否存在 sku_prop 中
      const findKey = skuAttrListKey.find(arr => {
        const arrMap = arr.split("|");
        return this.arrayIsEqual(arrMap, values);
      });
      const selectActiveKeys = Object.keys(activeKey);
      if (!findKey || selectActiveKeys.length === 0) {
        return {
          status: false,
          params: {}
        };
      }

      // 获取当前 sku_id 对应的属性组
      const findSkuItem = existSkuIdKey[findKey];
      // 判断是否可售
      if (findSkuItem.can_sell === 0) {
        return {
          status: false,
          title: "区域不可售",
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
  },
  methods: {
    arrayIsEqual(array1, array2) {
      return (
        array1.length == array2.length &&
        array1.every(v => array2.includes(Number(v)))
      );
    },
    disabledKey(attribute_value_id) {
      const sameAttrKey = Object.values(this.skuListMap.attrSameKey);
      const selectKeys = Object.values(this.activeKey);
      // 存在选项且小于可选项，不满足匹配规则，直接返回 false
      if (selectKeys.length < this.skuListMap.attrGroupList.length - 1) {
        return false;
      }
      // 获取同组数据
      const findSameKey = sameAttrKey.filter(arr => {
        return selectKeys.find(key => arr.includes(key));
      });

      const { existSkuIdKey } = this.skuListMap;
      // 取出 attribute_value_id
      const skuAttrListKey = Object.keys(existSkuIdKey);
      // 判断是否存在 SKU 组中
      const findKey = skuAttrListKey.some(arr => {
        const arrMap = arr.split("|");
        // 去除与当前 attribute_value_id 匹配的同组数据
        const aloneKeys = selectKeys.filter(key => {
          // 获取同组数据
          const sameGroupKey = findSameKey.filter(arr =>
            arr.includes(attribute_value_id)
          );
          return !sameGroupKey.some(skKey => skKey.includes(key));
        });

        return this.arrayIsEqual(arrMap, [
          ...new Set([...aloneKeys, attribute_value_id])
        ]);
      });

      // 不在同组属性中，属于 sku 中
      return (
        findSameKey.some(arr => !arr.includes(attribute_value_id)) &&
        !findKey
      );
    },
    handleClick(sku, tag) {
      if (this.disabledKey(tag.attribute_value_id)) {
        this.activeKey = {};
      }
      const key = tag.attribute_value_id;
      const attrKey = sku.attribute_id;
      this.$set(this.activeKey, attrKey, key);
    },
    handleAddToCart() {
      const params = this.validateCartStatus;
      if (params.status) {
        console.log(JSON.stringify(params, null, 2));
      }
    }
  }
};
</script>

<style lang="css">
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.cart-box {
  display: flex;
  flex-direction: column;
  height: 100vh;
  max-width: 750px;
  margin: 0 auto;
}

.cart-main {
  padding: 12px;
  flex: 1;
}

.cart-card {
  padding: 16px;
  box-shadow: 0px 2px 10px 0px rgba(93, 153, 240, 0.16);
  border-radius: 2px;
  margin-bottom: 8px;
}

.cart-card-container + .cart-card-container {
  margin-top: 24px;
}

.cart-card-title {
  font-size: 14px;
  color: rgba(0, 0, 0, 0.85);
  margin-bottom: 16px;
}

.cart-card-box {
  display: block;
}

.cart-card-item {
  display: inline-block;
  padding: 0 16px;
  height: 32px;
  line-height: 32px;
  font-size: 24rpx;
  color: rgba(0, 0, 0, 0.65);
  border-radius: 32px;
  border: 1px solid #f3f4f5;
  background-color: #fff;
}

.cart-card-item + .cart-card-item {
  margin-left: 16px;
}

.cart-card-item.active {
  color: #fff;
  background-color: #1890ff;
  border-color: #1890ff;
}

.cart-card-item.disabled:not(.active) {
  color: rgba(0, 0, 0, 0.065);
  background-color: #f3f4f5;
}

.cart-footer {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 45px;
  background-color: #408fff;
  background-image: linear-gradient(90deg, #408fff 0%, #42aaff 100%);
  box-shadow: 0px 6px 4px 0px rgba(100, 152, 223, 0.16);
  border-color: #408fff;
  color: #fff;
}
.cart-footer.disabled {
  background-color: #f3f4f5;
  background-image: none;
}

.cart-row {
  display: flex;
  align-items: flex-start;
}

.cart-row .cart-img {
  display: block;
  width: 120px;
  height: 120px;
  margin-right: 16px;
}

.cart-row .cart-name {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  word-break: break-all;
  font-size: 16px;
  color: rgba(0, 0, 0, 0.85);
  margin-bottom: 8px;
  width: 100%;
  overflow: hidden;
}

.cart-row .cart-money {
  color: #fe6b5d;
  font-weight: 600;
  font-size: 14px;
  margin-bottom: 8px;
}

.cart-row .cart-stock {
  color: rgba(0, 0, 0, 0.65);
  font-size: 14px;
}
</style>
