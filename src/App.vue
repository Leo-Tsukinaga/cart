<template>
  <div class="app-container">
    <EsHeader title="购物车案例"></EsHeader>
    <EsGoods
      v-for="item in goodslist"
      :key="item.id"
      :id="item.id"
      :title="item.goods_name"
      :thumb="item.goods_img"
      :price="item.goods_price"
      :count="item.goods_count"
      :checked="item.goods_state"
      @stateChange="onGoodsStateChange"
      @countChange="onGoodsCountChange"
    ></EsGoods>
    <EsFooter
      :amount="amount"
      :total="total"
      @fullChange="onFullStateChange"
    ></EsFooter>
  </div>
</template>

<script>
import EsHeader from "./components/es-header/EsHeader.vue";
import EsFooter from "./components/es-footer/EsFooter.vue";
import EsGoods from "./components/es-goods/EsGoods.vue";
export default {
  name: "MyApp",
  data() {
    return {
      // 商品列表的数据
      goodslist: [],
    };
  },
  components: {
    EsHeader,
    EsFooter,
    EsGoods,
  },
  methods: {
    // 获取商品列表数据的方法
    async getGoodsList() {
      const { data: res } = await this.$http.get("/api/cart");
      // console.log(res);
      if (res.status !== 200) {
        return alert("数据请求失败");
      }
      this.goodslist = res.list;
    },
    // 监听选中状态变化的时间
    onFullStateChange(e) {
      // console.log(e);
      this.goodslist.forEach((x) => (x.goods_state = e));
    },
    // 监听商品勾选状态变化的事件
    onGoodsStateChange(e) {
      const findresult = this.goodslist.find((x) => x.id == e.id);
      if (findresult) {
        findresult.goods_state = e.value;
      }
    },
    // 监听商品数量变化的事件
    onGoodsCountChange(e) {
      const findResult = this.goodslist.find((x) => x.id == e.id);
      if (findResult) {
        findResult.goods_count = e.value;
      }
    },
  },
  created() {
    this.getGoodsList();
  },
  computed: {
    // 以勾选商品的总价格
    amount() {
      let a = 0;
      this.goodslist
        .filter((x) => x.goods_state)
        .forEach((x) => {
          a += x.goods_price * x.goods_count;
        });
      return a;
    },
    // 以勾选商品的总数量
    total() {
      let t = 0;
      this.goodslist
        .filter((x) => x.goods_state)
        .forEach((x) => {
          t += x.goods_count;
        });
      return t;
    },
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
