<template>
  <div id = 'homeswiper'>
    <slot name='searchbar'>
       <transition name="fade" >
          <van-search v-if='searchTf' v-model="searchvalue" placeholder="请输入您想要查找的车型品牌"  class='topBarSearch'  left   background="#000000" :label='initVal'/>
       </transition>
    </slot>
    <slot name='buttonbar'>    
                <van-button  type='default' round class='searchbtn' v-if='!searchTf'><van-icon name="search" class='searchIcon'/><span id='sear'>搜索</span></van-button>
    </slot>
        <van-swipe class="my-swipe" :autoplay="2000"   indicator-color="orange" >
       <van-swipe-item  v-for='(item,index) of swiperimg' :key ='index'><img class="imgsheet" v-if=' swiperimg[0]' :src="`http://localhost:3000/img/${item}.jpg`" alt=""> </van-swipe-item>   
   </van-swipe>
     </div>
</template>;

<script>
import { getimg } from "@/network/ReqImg.js";
export default {
  components: {},
  data() {
    return {
      active: "1",
      swiperimg: [],
      searchvalue: "",
      wuhan: "武汉",
      searchTf: false
    };
  },computed: {
    initVal(){
    return  this.$store.state.initval.name
    }
  },
  mounted() {
    getimg().then(res => {
      res.forEach((item, index) => {
        this.swiperimg.push(item.ImageName);
      });
      window.addEventListener("scroll", this.handle);
    });
  },

  methods: {
    handle() {
      if (document.documentElement.scrollTop >= 100) {
        this.searchTf = true;
      } else {
        this.searchTf = false;
      }
    }
  }
};
</script>
 <style scoped lang='less'>
#homeswpier {
  width: 100vw;
}
.imgsheet {
  //轮播图图片大小样式
  width: 100vw;
  height: 24vh;
}
//搜索栏
.topBarSearch {
  position: fixed;
  top: 0;
  left: 0;
  float: right;
  padding: 0;
  width: 100%;
  height: (30/375 * 100) vh;
  z-index: 999;
}
.fade-enter-active,
.fade-leave-active {
  transition: all 1s ease;
}
.fade-enter,
.fade-leave-to {
  transform: translateX(360px);
  opacity: 0;
}
//搜索按钮
.searchbtn {
  height: 30px;
  width: 70px;
  font-size: 12px;
  display: flex;
  justify-content: center;
  position: fixed;
  right: 10px;
  top: 10px;
  padding: 0;
  z-index: 999;
  color: #fff;
  background: rgba(0, 0, 0, 0.493);
  .searchIcon {
    font-size: 16px;
    height: 100%;
    margin: 0 auto;
    display: inline-block;
  }
  .searchbtn span {
    float: left;
  }
}
#sear {
  margin-bottom: 3px;
  vertical-align: center;
}
</style>