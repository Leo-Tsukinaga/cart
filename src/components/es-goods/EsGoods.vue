<template>
  <div class="goods-container">
    <!-- 左侧图片区域 -->
    <div class="left">
      <div class="custom-control custom-checkbox">
        <input
          type="checkbox"
          class="custom-control-input"
          :id="id"
          :checked="checked"
          @change="onCheckBoxChange"
        />
        <label class="custom-control-label" :for="id">
          <!-- 商品的略缩图 -->
          <img :src="thumb" alt="商品图片" class="thumb" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="right">
      <!-- 商品名称 -->
      <div class="top">{{ title }}</div>
      <div class="bottom">
        <!-- 商品价格 -->
        <div class="price">￥{{ price.toFixed(2) }}</div>
        <!-- 商品数量 -->
        <div class="count">
          <EsCounter :num="count" :min="1" @numChange="getNumber"></EsCounter>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EsCounter from "../es-counter/EsCounter.vue";
export default {
  name: "EsGoods",
  emits: ["stateChange", "countChange"],
  props: {
    // 商品的id
    id: {
      type: [String, Number],
      required: true,
    },
    // 商品的名称
    title: {
      type: String,
      required: true,
    },
    // 商品的缩略图
    thumb: {
      type: String,
      required: true,
    },
    // 商品的单价
    price: {
      type: Number,
      required: true,
    },
    // 商品的数量
    count: {
      type: Number,
      required: true,
    },
    // 商品的勾选状态
    checked: {
      type: Boolean,
      required: true,
    },
  },
  components: {
    EsCounter,
  },
  methods: {
    // 传递商品的选中状态
    onCheckBoxChange(e) {
      this.$emit("stateChange", {
        id: this.id,
        value: e.target.checked,
      });
    },
    // 监听数量值的变化
    getNumber(num) {
      this.$emit("countChange", {
        id: this.id,
        value: num,
      });
    },
  },
};
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  display: flex;
  padding: 10px;
  // 左侧图片的样式
  .left {
    margin-right: 10px;
    // 商品图片
    .thumb {
      display: block;
      width: 100px;
      height: 100px;
      background-color: #efefef;
    }
  }
  // 右侧商品的名称、单价、数量的样式
  .right {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .top {
      font-weight: bold;
    }
    .bottom {
      display: flex;
      justify-content: space-between;
      align-items: center;
      .price {
        color: red;
        font-weight: bold;
      }
    }
  }
}
</style>
