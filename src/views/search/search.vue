<template>
  <div id="search">
    <!-- 搜索内容 GO -->
    <div class="search-result">
      <!-- 内容box GO -->
      <div class="search-result-img" v-for="(item,index) in list" :key="index" @click="resultClick(item.url,index)">
        <!-- 图片 GO -->
        <van-image width="7rem" height="9rem" :src="item.cover" lazy-load show-error show-loading />
        <!-- 图片 end -->
        <!-- 名字和最近更新 GO -->
        <div class="van-ellipsis">{{item.name}}</div>
        <div class="van-ellipsis">{{item.latest}}</div>
        <!-- 名字和最近更新 end -->
      </div>
      <!-- 内容box end -->
    </div>
    <!-- 搜索内容 end -->
  </div>
</template>

<script>
import { SearchData } from '@/api/network'
export default {
  data() {
    return {
      list: [], //搜索数据
    }
  },
  mounted() {
    this.list = this.$store.state.clist.list
  },
  //进入路由即触发 created()只触发一次
  activated() {
    //this.$nextTick()异步执行dom刷新
    this.$nextTick(() => {
      window.scrollTo(0, this.$homeScroll);
    })
  },
  //离开路由时
  beforeRouteLeave(to, from, next) {
    //全局变量homeScroll默认为0 离开页面时 记录当前的页面滚动值
    this.$homeScroll = document.documentElement.scrollTop || document.body.scrollTop;
    //需要执行next函数 否则不起作用
    next();
  },
  methods: {
    //点击进入详情页面
    resultClick(url, index) {
      this.$toast.loading({
        duration: 0, // 持续展示 toast
        message: '加载中...',
        forbidClick: false,
      });
      this.$router.push({ path: '/details', query: { url: url, collect: { list: this.list[index] } } })
    }
  },
  watch: {
    // 监听搜索内容的变化并赋值
    '$store.state.clist'(val, oldVal) {
      this.list = val.list
    }
  }
}
</script>

<style scoped>
#search {
  padding: 4rem 0rem 0rem 0rem;
  max-width: 50rem;
  min-width: 15rem;
  margin: auto;
}
.search-result {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-end;
  justify-content: space-evenly;
}
.search-result-img {
  flex: 0 0 1;
  width: 8rem;
  height: 12rem;
  text-align: center;
  text-align: left;
}
.van-ellipsis {
  width: 7rem;
  font-size: 1rem;
}
.van-ellipsis:nth-child(3) {
  font-size: 12px;
  margin: 0.5rem 0px 0px 0.2rem;
  color: #ccc;
}
</style>