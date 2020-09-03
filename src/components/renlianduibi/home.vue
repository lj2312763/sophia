<template>
  <div class="main">
    <div class="header_content">
      <div class="logo">
        <div class="logoPic" @click="goHome">
          <img src="../../assets/images/new_index/logo.png" alt="" srcset="">
        </div>
      </div>
      <div class="nav">元知人脸相似度对比</div>
    </div>
    <renlianduibi></renlianduibi>
  </div>
</template>

<script>
import renlianduibi from './renlianduibi';
export default {
  data () {
    return {
      isShow:false,
      info:null,
      currentIndex:-1
    }
  },
  created(){
    if(this.$route.query.title){
      if(this.$route.query.title==1){
          this.info={
            title:'元知人脸相似度对比',
            subTitle:this.$route.query.subTitle
          }
      }else{
        this.info=null;
      }
    }
  },
  watch:{
    $route(newVal,oldVal){
      if(newVal.query.title==1){
          this.info={
            title:'元知人脸相似度对比',
            subTitle:newVal.query.subTitle
          }
      }else{
         this.info=null;
      }
    }
  },
  components:{
    renlianduibi
  },
  mounted(){
    if(this.$route.path!="/experienceColl"){
      this.initScroll();
    }
  },
  methods:{
    initScroll() {
      let _this = this;
      window.addEventListener("scroll", function() {
         let pos = document.body.scrollTop || document.documentElement.scrollTop;
         if(pos>100){
           _this.isShow=true;
         }else{
           _this.isShow=false;
         }
      });
    },
  }
}
</script>
<style scoped lang="less">
.main{
  .header_content{
    width: 1200px;
    margin: auto;
    height: 60px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    .logo{
      width:127px;
      height: 100%;
      display: flex;
      align-items: center;
      font-size: 22px;
      color: #121C33;
      cursor: pointer;
      .logoPic{
        width:102px;
        img{
          width:100%;
          display: block;
        }
      }
    }
    .nav{
      position: absolute;
      height: 100%;
      width: 100%;
      display: flex;
      align-items: center;
      font-size: 18px;
      justify-content: center;
      color: #121C33;
    }

  }
}

</style>

