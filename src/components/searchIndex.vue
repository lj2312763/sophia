<template>
  <div class="tech_container">
    <div class="headerBg" id="particles-js">
      <div class="header_content">
        <div class="header_title">AI智能应用服务</div>
        <div class="header_desc">全栈的AI能力，在这里有你所要的一切</div>
        <div class="header_search">
            <Select v-model="search" placeholder='请输入您想要搜索的产品' filterable allow-create @on-create="handleCreate" @on-change="changeValue" :capture='false'>
                <Option v-for="item in searchList" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
            <div @click="searchClick(1)" class="search_img">
              <img :src="seachBtn">
            </div>
        </div>
        <div class="header_item">
            <div class="searchItem" v-for="item in linkList" :key="item.id"  @click="jumpDetail(item)">{{item.name}}</div>
        </div>
      </div>
    </div>
    <div class="tech_content" v-if="content.length>0">
        <div class="tech_content_list"  v-for="item in content" :key="item.id" @click="jump(item)">
            <div class="title" v-html="item.title"></div>
            <div class="intro">{{item.description}}</div>
        </div>
    </div>
    <div class="empty" v-else>
      <Spin v-if="isloading" style="margin:80px 0;">
          <Icon type="ios-loading" size=76 class="demo-spin-icon-load"></Icon>
          <div>加载中...</div>
      </Spin>
    </div>
    <div class="empty" v-if="content.length==0&&isloading==false">
      <div class="emptyPic">
          <img :src="sorryPic">
      </div>
      <p>抱歉，没有搜索到相关结果。</p>
    </div>
    <div style="margin:auto;margin-bottom:50px;margin-top:40px;text-align:center"  v-if="content.length>0">
       <Page :total="paging.total" :current="paging.index" @on-change="pagingChange"></Page>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
       isloading:true,
       sorryPic:require('@/assets/images/new_nav/sorry.png'),
       panel_hot:require('@/assets/images/new_nav/hot.png'),
       //search:'',
       search:this.$route.query.search,
       seachBtn:require('@/assets/images/new_index/search.png'),
       searchList:[
         {label:"图像识别",value:'图像识别'},
         {label:"词法分析",value:'词法分析'},
         {label:"短语音识别",value:'短语音识别'},
         {label:"人脸识别",value:'人脸识别'},
         {label:"卡证文字识别",value:'卡证文字识别'},
       ],
      linkList:[
        {name:'文本审核',id:1,path:"/serviceDetail",oneId:5,twoRouteId:'5.3',routeId:'5.3'},
        {name:'机器翻译',id:2,path:"/serviceDetail",oneId:5,twoRouteId:'5.4',routeId:'5.4.1'},
        {name:'语音合成',id:3,path:"/serviceDetail",oneId:1,twoRouteId:'1.1',routeId:'1.1.4'},
        {name:'人脸识别',id:4,path:"/serviceDetail",oneId:6,twoRouteId:'6.1',routeId:'6.1.1'},
        {name:'智能推荐',id:5,path:"/serviceDetail",oneId:7,twoRouteId:'7.4',routeId:'7.4'}
        ],
        paging: {
          index: 1,
          size: 10,
          total: 0
        },
        content:[],
    };
  },
  created(){
    this.search=this.$route.query.search?this.$route.query.search:'';
    this.searchList.push({label:this.search,value:this.search})
    this.initData();
  },
  mounted() {
    particlesJS.load("particles-js", "./static/particles.json");
  },
  methods:{
     jumpDetail(item){
      let routeData = this.$router.resolve({
        path:item.path,
        query:{
          oneId:item.oneId,
          twoRouteId:item.twoRouteId,
          routeId:item.routeId,
          title:2,
          subTitle:item.name
        }
      });
      window.open(routeData.href, '_blank');
    },
     jump(item){
       let path=item.routePath.split('/');
      if(item.routeId =='5.5.2'){
        this.$router.push({
          path:'/opinionAnalysisDetail',
          title:2,
          subTitle:"舆情分析"
        });
      }
       if(item.category=="11111"){
          var title = item.title.replace(/<span style="color: #FE5560;">/g,'').replace(/<\/span>/g,'');
          this.$router.push({
            path:'/serviceDetail',
            query:{
              oneId:path[1],
              twoRouteId:path[2],
              routeId:item.routeId,
              title:2,
              subTitle:title
            }
          });
        }else{
          var title = item.title.replace(/<span style="color: #FE5560;">/g,'').replace(/<\/span>/g,'');
          this.$router.push({
            path:'/serviceDetailSimple',
            query:{
              oneId:path[1],
              twoRouteId:path[2],
              routeId:item.routeId,
              title:2,
              subTitle:title
            }
          });
        }
     },
     changeValue(val){
       if(val){
          this.searchClick(val);
       }
     },
     handleCreate (val) {
          this.searchList.push({
              value: val,
              label: val
          });
      },
      initData(){
              this.content=[];
              this.isloading=true;
              this.$axios({
                  method: "get",
                  url: window.config.url + "/sitemgr/page/searchinfo",
                  params:{
                    category:'',
                    keyword:this.search,
                    pageIndex:this.paging.index,
                    pageSize:this.paging.size,
                  }
              }).then(res => {
                  if(res.status==200){
                     this.isloading=false;
                    let result=res.data.data.data;
                    this.paging.total=res.data.data.totalNum;
                    this.paging.index = res.data.data.pageNum;
                    console.log(this.paging.total)
                    result.forEach(item=>{
                      var reg='/'+this.search+'/g';
                      let _title= item.title+"（"+item.parentModule.moduleName+"）";
                      item.title=_title.replace(eval(reg),'<span style="color: #FE5560;">'+this.search+'</span>');
                    })
                    this.content=result;
                  }
              }).catch(err=>{
                 this.isloading=false;
                  console.log(err);
              });
      },
     pagingChange(val){
      this.paging.index = val;
      document.documentElement.scrollTop=0;
      this.initData();
     },
     searchClick(val){
      var searText1;
      var searText = document.getElementsByClassName('ivu-select-default')[0].children[0].children[1].children[1].value;
      if(val==1){
        searText1 = searText;
        this.search=searText1;
        this.handleCreate(searText1)
        // this.searchList.push({label:searText1,value:searText1})
      }
      else{
        searText1 = val;
      }
      this.initPaging();
      this.initData();
     },
     initPaging() {
      this.paging.index = 1;
      this.paging.size = 10;
     },
  }
};
</script>
<style lang="less">
.tech_container{
  .ivu-page-item:hover {
      border-color: none;
      background: #03A971;
  }
  .ivu-page-item-active a, .ivu-page-item-active:hover a {
    color: #fff;
  }
  .ivu-page-item:hover a {
      color: #fff;
  }
  .ivu-page-item-active {
      border-color: #03A971;
      background: #03A971;
  }
  .ivu-page-item:hover {
      border-color: #03A971;
  }
  .ivu-page-next:hover, .ivu-page-prev:hover {
      border-color: #03A971;
      background: #03A971;
  }
  .ivu-page-next:hover a, .ivu-page-prev:hover a {
      color: #fff;
  }
}
</style>
<style scoped lang="less">
.headerBg {
  height: 280px;
  background: url("../assets/images/new_nav/headerBg.png") no-repeat center center;
  background-size: 100% 100%;
  position: relative;
  .header_content {
    position: absolute;
    width: 600px;
    left: 0;
    right: 0;
    top: 65px;
    margin: auto;
    .header_title {
      font-size: 36px;
      color: #ffffff;
      text-align: center;
      margin-bottom: 15px;
    }
    .header_desc {
      font-size: 18px;
      color: #ffffff;
      text-align: center;
      margin-bottom: 25px;
    }
    .header_search {
      width: 100%;
      height: 45px;
      margin-bottom: 10px;
      display: flex;
      align-items: center;
      position: relative;
      /deep/.ivu-select-default {
        height: 100% !important;
        .ivu-select-selection {
          height: 100%;
          padding-left:7px;
          &>div{
             height: 100%;
              .ivu-select-input {
                height: 100% !important;
                line-height: 44px;
              }
          }
          .ivu-select-selected-value {
            line-height: 44px;
            height: 100%;
          }
          .ivu-select-placeholder {
            line-height: 44px;
            color: #B8BECC;
            height:100%;
          }
          .ivu-select-arrow {
            display: none;
          }
        }
        /deep/.ivu-select-dropdown {
          /deep/.ivu-select-dropdown-list {
            .ivu-select-item {
              padding: 11px 10px;
              color: #7A8499;
            }
            .ivu-select-item-focus{
              background: none;
              color: #03a971;
            }
            .ivu-select-item:hover {
              color: #03a971;
              background: none;
            }
          }
        }
      }
      .search_img{
        position: absolute;
        height: 100%;
        width: 40px;
        right: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 12;
        img {
          width: 16px;
          cursor: pointer;
        }
      }
      .search_img:hover{
        cursor: pointer;
      }
    }
    .header_item{
      display: flex;
      justify-content: center;
      .searchItem{
        font-size: 14px;
        color: #FFFFFF;
        margin-right: 30px;
        cursor: pointer;
      }
    }
  }
}
.tech_content{
  width:1200px;
  margin:auto;
  padding-top: 30px;
  .tech_content_list{
    cursor: pointer;
    margin-bottom: 40px;
    .title{
      font-size: 18px;
      color: #000015;
      margin-bottom: 10px;
    }
    .intro{
      font-size: 14px;
      color: #3D4966;
      line-height: 24px;
      margin-bottom: 10px;
    }
    .source{
      font-size: 14px;
      color: #7A8499;
    }
  }
}
.empty{
   width:1200px;
   margin:auto;
   .emptyPic{
     width:200px;
     margin:auto;
     margin-top:80px;
     margin-bottom: 20px;
     img{
       width:100%;
       display: block;
     }
   }
    p{
        font-size: 16px;
        color: #3D4966;
        text-align: center;
        margin-bottom: 68px;
     }
}

</style>
