<template>
  <div>
    <MyHeader title="购物车"></MyHeader>
    <div style="padding-top: 45px">
      <component :is="comName"></component>
    </div>
    <MyTabBar :list="tabList" @changeFn="changeFn"></MyTabBar>
  </div>
</template>

<script>
import MyHeader from "./components/MyHeader.vue";
import MyTabBar from "./components/MyTabBar.vue";
import MyGoodsList from "./views/MyGoodsList.vue";
import MyUserInfo from "./views/MyUserInfo.vue";
import MyGoodsSearch from "./views/MyGoodsSearch.vue";
export default {
  name: "App",
  data() {
    return {
      comName: "MyGoodsList",
      tabList: [
        {
          iconText: "icon-shangpinliebiao",
          text: "商品列表",
          componentName: "MyGoodsList",
        },
        {
          iconText: "icon-sousuo",
          text: "商品搜索",
          componentName: "MyGoodsSearch",
        },
        {
          iconText: "icon-user",
          text: "我的信息",
          componentName: "MyUserInfo",
        },
      ],
    };
  },
  components: {
    MyHeader,
    MyTabBar,
    MyGoodsList,
    MyUserInfo,
    MyGoodsSearch,
  },
  methods: {
    changeFn(item) {
      this.comName = item.componentName;
    },
  },
};
</script>
