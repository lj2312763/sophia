<template>
  <div class="experience">
    <Spin fix v-if="isloading">
      <Icon type="ios-loading" size="76" class="demo-spin-icon-load"></Icon>
      <div>识别中...</div>
    </Spin>


    <div class="total_title">证书识别</div>
    <div class="fn_container">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                :show-upload-list="false"
                type="drag"
                :before-upload="chengduiUpload"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="chengDuiPic" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过1M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="chengduiMsg">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { imgPreviewBase64, getImgSize} from "@/assets/js/imgPreviewBase64";
export default {
  data() {
    return {
      chengDuiPic:require("@/assets/images/piaoju/zheng1.png"),
      chengduiMsg:``,
      isloading: false,
      whFlag: true,
    };
  },
  created(){
  },
  mounted() {
    let _this = this;
    $(document).ready(function () {
        _this.chengduiMsg=`
                    <p>姓名：王立云</p>
                    <p>查验结果：二级注册建造师</p>
                   `;
    });
  },
  methods: {
    chengduiUpload(file){
        let _this = this;
        imgPreviewBase64(_this, file, function(base64){
          _this.chengDuiPic=base64; 
           getImgSize(_this.chengDuiPic).then(res => {
            _this.whFlag = res.flag;
          });   
        })
         let formData = new FormData(); 
         formData.append('files', file);
         formData.append('languageType', 'CHN_ENG');
         this.isloading=true;
        this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "post",
          url: window.config.url+"/ai/text/general",
          data: formData,
        }).then(res => {
           this.isloading=false;
          if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            if(res.data.data){
              let result=res.data.data.words_result;
              let currentIndex=-1;
              let currentIndex1=-1;
              let name='';
              let cardNo='';
              let cerNo='';
              result.forEach((item,index)=>{
                if(item.words.includes('姓名')){
                  currentIndex=index+1;
                }
                if(index==currentIndex){
                  name=item.words;
                }
                if(item.words.includes('证件号码')){
                  cardNo=item.words.split(':')[1];
                }
                if(item.words.includes('注册编号')){
                  currentIndex1=index+1;
                  cerNo=item.words.split('证书编号')[1];
                }
                 if(index==currentIndex1){
                  cerNo=item.words;
                }
              })
              this.realData(name,cardNo,cerNo)
            }
          }
          else{
            this.$Message.error('请求失败');
          }
        }).catch(err=>{
           this.isloading=false;
          console.log(err);
        });
    },
    realData(name,cardNo,cerNo){
        this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "get",
          //url: window.config.url+"/ai/architect/identify",
          url: "https://test.yzsmart.top:38443/ai/architect/identify",
          params: {
              ry_name:name,//姓名
              reg_seal_code:cerNo,//证件号
              ry_cardno:cardNo//身份证号
          },
        }).then(res => {
           this.isloading=false;
          if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            if(res.data.data){
                let result=res.data.data?res.data.data[0]:null;
                if(result){
                  this.chengduiMsg=`
                          <p>姓名：${result.RY_NAME}</p>
                          <p>查验结果：${result.REG_TYPE_NAME}</p>
                        `;
                }else{
                  this.chengduiMsg='查无结果';
                }
            }
          }
          else{
            this.$Message.error('请求失败');
          }
        }).catch(err=>{
           this.isloading=false;
          console.log(err);
        });

    },
  },
};
</script>
<style scoped lang="less">
.experience {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
  position: relative;
  .total_title {
    width: 100%;
    height: 90px;
    text-align: center;
    line-height: 90px;
    font-size: 24px;
    margin-top: 20px;
    margin-bottom: 20px;
  }
  .fn_container {
    width: 90%;
    margin: auto;
    margin-bottom: 60px;
    /deep/.ivu-radio-default {
      font-size: 14px;
      color: #121c33;
      margin-bottom: 10px;
      label {
        margin-right: 20px;
        .ivu-radio-inner::after {
          border-color: #03a971;
        }
        .ivu-radio-checked .ivu-radio-inner {
          border-color: #03a971;
        }
        .ivu-radio-inner::after {
          background-color: #03a971;
        }
      }
    }
    p {
      font-size: 14px;
      color: #7a8499;
      text-align: justify;
      line-height: 24px;
      margin-top: 10px;
    }
    .content {
      width: 100%;
      display: flex;
      justify-content: space-between;
      .title {
        font-size: 18px;
        color: #121c33;
        margin-bottom: 20px;
      }
      .left {
        width: 50%;
        .left_box {
          height: 350px;
          border: 1px solid #ebecf0;
          .left_up {
            width: 100%;
            height: 100%;
            background: rgba(192, 204, 218, 0.1);
            position: relative;
            .imgBox {
              display: inline-block;
              margin: 20px 50px;
              .imgPic {
                width: 60px;
                margin: auto;
                position: relative;
                img {
                  width: 100%;
                  display: block;
                }
                .del {
                  position: absolute;
                  right: -8px;
                  top: -3px;
                  width: 15px;
                  cursor: pointer;
                }
              }

              p {
                text-align: center;
              }
            }
            .pic {
              width: 100%;
              &.whFlag {
                height: 100%;
                display: flex;
                align-items: center;
                justify-content: center;
                overflow: hidden;
                img {
                  width: -webkit-fill-available;
                  height: 100%;
                }

                canvas {
                  width: auto;
                  height: 100%;
                }
              }
              img {
                width: 100%;
                display: block;
              }
            }
            .intro {
              display: flex;
              justify-content: space-between;
              padding: 0 15px;
              align-items: center;
              opacity: 0.85;
              background: #121c33;
              position: absolute;
              bottom: 0;
              left: 0;
              width: 100%;
              height: 50px;
              .text {
                font-size: 12px;
                color: #ffffff;
                text-align: justify;
              }
            }
          }
          .ivu-upload {
            /deep/.ivu-upload-drag {
              height: 100%;
            }
            /deep/.ivu-upload-select {
              .ivu-btn-default {
                background: #03a971 !important;
                border: none !important;
                span {
                  color: #ffffff;
                }
              }
            }
            /deep/.ivu-upload-list {
              display: none;
            }
          }
        }
        .option {
          margin-top: 10px;
          display: flex;
          flex-wrap: wrap;
          .tag {
            padding: 5px 10px;
            font-size: 14px;
            background: rgba(0, 0, 0, 0.6);
            float: left;
            color: #fff;
            margin-right: 10px;
            margin-bottom: 20px;
            cursor: pointer;
            border-radius: 3px;

            &.tagActive {
              background: #03a971;
            }
          }

          .tag1 {
            width: 150px;
            margin-right: 20px;
            position: relative;
            border:grey 1px solid;
            margin-bottom: 20px;
            max-height: 70px;
            overflow: hidden;
            cursor: pointer;
            float: left;
            img {
              width: 100%;
              display: block;
            }
            .typeText {
              width: 100%;
              height: 25px;
              line-height: 25px;
              text-align: center;
              position: absolute;
              left: 0;
              bottom: 0;
              opacity: 0.85;
              background: #121c33;
              font-size: 14px;
              color: #ffffff;
            }
            &.tagActive1 {
              border: #03a971 2px solid;
            }
          }
        }
      }
      .right {
        flex: 1;
        margin-left: 20px;
        .right_box {
          overflow: auto;
          font-size: 14px;
          color: #121c33;
          letter-spacing: 0;
          line-height: 24px;
          padding: 10px;
          height: 350px;
          background: rgba(192, 204, 218, 0.1);
          border: 1px solid #ebecf0;
          .table{
            border-collapse: collapse;
            tr{
              th,td{
                border: grey 1px solid;
              }
            }
          }
        }
      }
    }
  }
}
</style>

