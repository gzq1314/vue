<template>
  <div>
    <!-- 头部 -->
    <div class="box">
      <h3 v-text="msg"></h3>
    </div>
    <!-- 搜索 -->
    <van-search placeholder="搜索饿了么商家、商品名称" v-model="value" background="#0085ff" />
    <!-- 商品类型 -->
    <van-grid>
      <van-grid-item v-for="(k,index) in entriesComputed" :key="index" :icon="k.image_hash" :text="k.name" />
    </van-grid>
    <!-- 广告 -->
    <div class="nav">
      <h4>品质套餐</h4>
      <p>搭配齐全吃得好</p>
      <div>立即抢购 ></div>
      <img src="https://fuss10.elemecdn.com/e/ee/df43e7e53f6e1346c3fda0609f1d3png.png?imageMogr/format/webp/thumbnail/!282x188r/gravity/Center/crop/282x188/" alt="">
    </div>
    <!-- 下拉菜单 -->
    <van-divider :style="{ color: '#222' }">---&nbsp;&nbsp;&nbsp;推荐商家&nbsp;&nbsp;&nbsp;---</van-divider>
    <van-dropdown-menu>
      <van-dropdown-item v-model="value1" :options="menu|handleMenu" />
      <van-dropdown-item v-model="value1" :options="menu|handleMenu" />
    </van-dropdown-menu>
    <!-- 列表页 -->
    <van-card v-for="(c,index) in list" :key="index" num="2" :price="c.restaurant.piecewise_agent_fee.description" :desc="c.restaurant.flavors[0].name" :title="c.restaurant.name" :thumb="c.restaurant.image_path" />
    <!-- 底部 -->
    <van-tabbar v-model="active">
      <van-tabbar-item icon="home-o">首页</van-tabbar-item>
      <van-tabbar-item icon="search">发现</van-tabbar-item>
      <van-tabbar-item icon="friends-o">订单</van-tabbar-item>
      <van-tabbar-item icon="setting-o">我的</van-tabbar-item>
    </van-tabbar>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value: "",
      msg: "广州市人名政府",
      //宫格
      entries: [],
      //下拉菜单
      value1: 0,
      menu: [],
      list: [],
      active: ""
    };
  },
  computed: {
    entriesComputed() {
      //切割成八个宫格
      return this.entries.slice(0, 8);
    }
  },
  async created() {
    //获取数据
    let data = await this.$axios(
      "https://www.easy-mock.com/mock/5d40257feda3776c6f9bf2da/example/entires"
    );
    this.entries = data.data[0].entries;
    //获取下拉菜单
    let menu = await this.$axios(
      "https://www.easy-mock.com/mock/5d40257feda3776c6f9bf2da/example/batch"
    );
    this.menu = menu.data.outside.inside_sort_filter;
    //商家列表
    let list = await this.$axios(
      "https://www.easy-mock.com/mock/5d40257feda3776c6f9bf2da/example/list"
    );
    this.list = list.data.items;
  },
  filters: {
    handleMenu(menu) {
      let newMenu = [];
      menu.forEach((element, index) => {
        // 往对象里面新增一个text属性值
        element.text = element.name;
        element.value = index;
        //构造一个新的数组
        newMenu.push(element);
      });
      return newMenu;
    }
  }
};
</script>
<style scoped>
#app {
  height: 100%;
}
.box {
  height: 40px;
  background: #0085ff;
}
.box h3 {
  position: absolute;
  top: -4px;
  left: 30px;
  font-size: 18px;
  color: #fff;
  font-weight: 700;
}
.nav {
  width: 100%;
  height: 90px;
  background: linear-gradient(0deg, #f4f4f4 5%, #fafafa 95%);
  padding: 20px 0 0 20px;
  position: relative;
}
.nav h4 {
  margin: 0;
  padding: 0;
  height: 20px;
  margin-bottom: 5px;
}
.nav p {
  margin: 0;
  padding: 0;
  height: 15px;
  margin-bottom: 10px;
  font-size: 14px;
  color: #777;
}
.nav img {
  width: 144px;
  height: 94px;
  position: absolute;
  right: 10%;
  top: 10%;
}
.nav div {
  font-size: 12px;
  color: #af8260;
  font-weight: 700;
}
</style>
