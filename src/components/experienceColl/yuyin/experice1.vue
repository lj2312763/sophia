<template>
  <div class="experience">
      <!-- 短语音识别 -->
    <div class="duanyuyin_experience_container" v-if="routeId=='1.1.1'">
      <div class="type">
          <Select v-model="language" placeholder='中文' @on-change="changeLanguage">
              <Option v-for="item in languageList" :value="item.value" :key="item.value">{{ item.label }}</Option>
          </Select>
        </div>
      <div class="top">
        
        <div class="icon" style="">
          <div class="luzhi" style="width:15px;">
             <img src="../../../assets/images/aiSmartAppDetail/luzhi.png" alt="">
          </div>
          <div v-if="isWork" class="timeline" style="width:293px; margin:0 10px">
            <img src="../../../assets/images/aiSmartAppDetail/timeLine.gif" alt="">
          </div>
          <span class="time" v-if="isWork" style="color:#fff;">00:{{upTime}} / 00:10</span>
        </div>
        <div class="times">
            <i-button type="success"  @click="startRecord">{{buttonMsg}}</i-button>
        </div>
      </div>
      <div class="bottom">
        {{transMsg}}
      </div>
    </div>

    <!-- 实时语音识别 -->
    <div class="duanyuyin_experience_container" v-if="routeId=='1.1.2'">
      <div class="top">
        <div class="icon" style="">
          <div class="luzhi" style="width:15px;">
             <img src="../../../assets/images/aiSmartAppDetail/luzhi.png" alt="">
          </div>
          <div v-if="isWork" class="timeline" style="width:293px; margin:0 10px">
            <img src="../../../assets/images/aiSmartAppDetail/timeLine.gif" alt="">
          </div>
          <span class="time" v-if="isWork" style="color:#fff;">00:{{upTime}} / 00:10</span>
        </div>
        <div class="times">
            <i-button type="success"  @click="startRecord1">{{buttonMsg}}</i-button>
        </div>
      </div>
      <div class="bottom">
        {{transMsg}}
      </div>
    </div>

    <!-- 录音文件识别 -->
    <div class="fn_container" v-if="routeId=='1.1.3'">
      <div class="content">
        <div class="left">
          <div class="title">上传音频：</div>
          <div class="left_box">
            <!-- :format="['pcm','mp3','wav']" -->
            <Upload
                ref="upload"
                type="drag"
                :on-success="handleSuccess"
                :before-upload="handleBeforeUpload"
                :on-format-error="handleFormatError"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                >
                <div class="left_up">
                    <div class="pic">
                      <img src="../../../assets/images/yuyin/voice.png" alt="">
                    </div>
                    <div class="intro">将文件拖到此处，或 <span>点击上传</span></div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box">
             {{anyMsg}}
          </div>
        </div>
      </div>
      <p>单个文件支持时长2分钟以内、5M以下，音频格式支持：pcm，wav，vox，Alaw，Ulaw</p>
    </div>

      <!-- 语音评测 -->
    <div class="duanyuyin_experience_container" v-if="routeId=='1.2.1'">
      <div class="info_text_type">
        <div class="info_text">请朗读以下文字：</div>
        <div class="type">
            <Select v-model="source"  @on-change="changeVal">
                <Option v-for="item in langList" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
        </div>
      </div>
      <div style="background: rgba(192,204,218,0.10);height:180px;border: 1px solid #EBECF0;padding:15px;margin-bottom:20px">
        {{demoText}}
      </div>
      <div class="top">
        <div class="icon" style="">
          <div class="luzhi" style="width:15px;">
             <img src="../../../assets/images/aiSmartAppDetail/luzhi.png" alt="">
          </div>
          <div v-if="isWork3" class="timeline" style="width:293px; margin:0 10px">
            <img src="../../../assets/images/aiSmartAppDetail/timeLine.gif" alt="">
          </div>
          <span class="time" v-if="isWork3" style="color:#fff;">00:{{upTime3}} / 00:10</span>
        </div>
        <div class="times">
            <i-button type="success"  @click="startRecord2">{{buttonMsg3}}</i-button>
        </div>
      </div>
      <div style="text-align:center;margin:20px 0;">
          <i-button type="success"  @click="sendRecord2(demoText)">开始测评</i-button>
      </div>
      <div class="bottom" style="height:auto">
        总分：{{result}}
      </div>
    </div>
     <!-- 歌曲识别 -->
    <div class="duanyuyin_experience_container" v-if="routeId=='1.2.2'">
       <div class="info_text_type">
        <div class="info_text"></div>
        <div class="type">
            <Select v-model="source1">
                <Option v-for="item in singList" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
        </div>
      </div>
      <div class="top">
        <div class="icon" style="">
          <div class="luzhi" style="width:15px;">
             <img src="../../../assets/images/aiSmartAppDetail/luzhi.png" alt="">
          </div>
          <div v-if="isWork4" class="timeline" style="width:293px; margin:0 10px">
            <img src="../../../assets/images/aiSmartAppDetail/timeLine.gif" alt="">
          </div>
          <span class="time" v-if="isWork4" style="color:#fff;">00:{{upTime4}} / 00:10</span>
        </div>
        <div class="times">
            <i-button type="success"  @click="startRecord3">{{buttonMsg4}}</i-button>
        </div>
      </div>
      <p style="margin:15px 0;font-size: 18px;color: #121C33;">分析结果：</p>
      <div class="bottom" >
        <span v-if="result1">{{result1.song}} —— {{result1.singer}}</span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['routeId'],
    data() {
      return {
        result1:'',
        result:'',
        demoText:'将文字信息转化为声音信息，',
        source:'zh_cn',
        source1:'2',
        singList:[
          {
            label:'哼唱识别',
            value:'1'
          },
          {
            label:'原声识别',
            value:'2'
          }
        ],
        langList:[
          {
            label:'中文',
            value:'zh_cn'
          },
          {
            label:'英文',
            value:'en_us'
          }
        ],
        isWork:false,
        isWork3:false,
        isWork4:false,
        upTime:10,
        upTime3:10,
        upTime4:10,
        numTime:0,
        numTime4:0,
        transMsg:"请说出你想说的话...",
        buttonMsg:"开始录音",
        buttonMsg3:"开始录音",
        buttonMsg4:"开始识别",
        flag:false,
        flag3:false,
        flag4:false,
        recorder:null,
        recorder1:null,
        recorder2:null,
        recorder3:null,
        recorder4:null,
        anyMsg:"",
        capkey:'',
        language:'',
        languageList:[
          {
            label:'中文',
            value:''
          },
          {
            label:'英文',
            value:'asr.cloud.freetalk.english'
          },
          {
            label:'粤语',
            value:'asr.cloud.freetalk.cantonese'
          }
        ]
      }
    },
    created() {
    },
    mounted(){
      this.recorder = new Recorder({
          sampleBits: 16,         // 采样位数，支持 8 或 16，默认是16
          sampleRate: 16000,      // 采样率，支持 11025、16000、22050、24000、44100、48000，根据浏览器默认值，我的chrome是48000
          numChannels: 1,         // 声道，支持 1 或 2， 默认是1
          compiling: false,       // 是否边录边转换，默认是false
      });
      this.recorder4 = new Recorder({
          sampleBits: 16,         // 采样位数，支持 8 或 16，默认是16
          sampleRate: 16000,      // 采样率，支持 11025、16000、22050、24000、44100、48000，根据浏览器默认值，我的chrome是48000
          numChannels: 1,         // 声道，支持 1 或 2， 默认是1
          compiling: false,       // 是否边录边转换，默认是false
      });
      this.recorder3 = new Recorder({
          sampleBits: 16,         // 采样位数，支持 8 或 16，默认是16
          sampleRate: 16000,      // 采样率，支持 11025、16000、22050、24000、44100、48000，根据浏览器默认值，我的chrome是48000
          numChannels: 1,         // 声道，支持 1 或 2， 默认是1
          compiling: false,       // 是否边录边转换，默认是false
      });
    },
    methods:{
      changeLanguage(v){
        this.capkey = v
      },
      changeVal(val){
        if(val=='en_us'){
          this.demoText="Firefighters take part in an emergency rescue drill in a forest in Taian city, Shandong province, on Feb 24, 2019. This is the country's largest joint air-ground drill with around 2,000 rescuers, seven helicopters and vehicles, and over 1,200 firefighting equipment taking part in the exercise.";
        }else{
          this.demoText='将文字信息转化为声音信息，给应用配上嘴巴。我们提供了众多极具特色的发音人供您选择。';
        }
        console.log(this.demoText)
      },
      handleSuccess(){},
      handleBeforeUpload(file){
        console.log(file)
        let _this=this;
         var reader = new FileReader();
          reader.readAsDataURL(file);
          reader.onload = function(e){
              // target.result 该属性表示目标对象的DataURL
              
               let data=e.target.result;
               var mimeString = data.split(',')[0].split(':')[1].split(';')[0];//图片类型
               data=data.split(',')[1];
              //转化为二进制
              data=window.atob(data);
              var ia = new Uint8Array(data.length);
              for (var i = 0; i < data.length; i++) {
                  ia[i] = data.charCodeAt(i);
              }
              var blob=new Blob([ia],{type:"audio/amr"});
              console.log(blob);
              let formData = new FormData(); //创建form对象
              formData.append('files', blob);//
              _this.$axios({
                headers: {
                    'Accept': '*/*',
                    'Content-Type': 'multipart/form-data'
                },
                method: "post",
                url: window.config.url+"/ai/voice/asr?audioFormat=amr",
                data: formData,
              }).then(res => {
                if(res.data.code == 200000){
                  if(!res.data.data){
                    this.$Message.error('返回数据为空');
                    return;
                  }
                  _this.anyMsg=res.data.data.ResponseInfo.Result.Text;
                }
                else{
                  this.$Message.error('请求失败');
                }
              }).catch(err=>{
                console.log(err);
              });
          }
      },
      handleFormatError (file) {
        //console.log(file)
          // this.$Notice.warning({
          //     title: 'The file format is incorrect',
          //     desc: 'File format of ' + file.name + ' is incorrect, please select jpg or png.'
          // });
      },
      startRecord3(){
        if (navigator.mediaDevices.getUserMedia) {
          const constraints = { audio: true };
          navigator.mediaDevices.getUserMedia(constraints).then(
            stream => {
              console.log("授权成功！");
            },
            () => {
              console.error("授权失败！");
            }
          );
        } else {
            alert("浏览器不支持 getUserMedia");
        }
        this.flag4=!this.flag4;
        //当为true，暂停状态
        var timer=null;
        if(this.flag4){
          this.upTime4=10;
          try {
            this.recorder4.start();
          } catch (error) {
            alert("您的浏览器不支持");
            return;
          }
          this.isWork4=true;
          this.buttonMsg4="结束识别";
          timer=setInterval(()=>{
          if(this.upTime4==0){
              this.buttonMsg4="开始识别";
              this.flag4=false;
              clearInterval(timer)
              this.sendRecord3();
              this.isWork4=false;
            }else{
              this.upTime4--;
              this.upTime4="0"+this.upTime4;
              if(this.flag4){
                this.buttonMsg4="结束识别";
              }else{
                this.isWork4=false;
                this.sendRecord3();
                clearInterval(timer)
                this.buttonMsg4="开始识别";
              }
            }
          },1000)
        }else{
          this.buttonMsg4="开始识别";
          if(timer){
              clearInterval(timer)
          }
        }        
      },
      // startRecord3(){
      //   let _this=this;
      //   var canRecording=ZCRecorder.canRecording;
      //   if(!canRecording){
      //     alert("不支持录音")
      //     return;
      //   }
      //     ZCRecorder.get(function(res){
      //       _this.recorder4=res;
      //       _this.flag4=!_this.flag4;
      //       //当为true，暂停状态
      //       var timer=null;
      //       if(_this.flag4){
      //          _this.recorder4.start();
      //         _this.upTime4=10;
      //         _this.numTime4=0;
      //         _this.isWork4=true;
      //         _this.buttonMsg4="结束识别";
      //         let _data=null;
      //         timer=setInterval(()=>{
      //         if(_this.upTime4==0){
      //             _this.buttonMsg4="开始识别";
      //             _this.flag4=false;
      //             clearInterval(timer)
      //             _data=_this.recorder4.getBlob();
      //             _this.sendRecord1(_data);
      //             _this.isWork4=false;
      //           }else{
      //             _this.numTime4++;
      //             _this.upTime4--;
      //             _this.upTime4="0"+_this.upTime4;
      //             _data=_this.recorder4.getBlob();
      //             _this.sendRecord3(_data);
      //             if(_this.flag4){
      //               _this.buttonMsg4="结束识别";
      //             }else{
      //               _this.isWork4=false;
      //               clearInterval(timer)
      //                _data=_this.recorder4.getBlob();
      //                _this.recorder4.stop();
      //               _this.sendRecord3(_data);
      //               _this.buttonMsg4="开始识别";
      //             }
      //           }
      //         },2000)
      //       }else{
      //         _this.buttonMsg4="开始识别";
      //         if(timer){
      //             clearInterval(timer)
      //         }
      //       }        

      //   },function(error){
      //     console.log(error)
      //   });
       
      // },
      sendRecord3(val){
        this.recorder4.stop();
        let wav=this.recorder4.getWAVBlob();
        let formData = new FormData(); //创建form对象
        formData.append('files', wav);//
        this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/voice/songDiscern",
          data: formData,
        }).then(res => {
          if(res.data.code == 200000){
            if(!res.data.data){
              this.$Message.error('返回数据为空');
              return;
            }
            this.result1=res.data.data.data[0];
          }
          else{
            this.$Message.error('请求失败');
          }
        }).catch(err=>{
          console.log(err);
        });
      },
      startRecord2(){
        if (navigator.mediaDevices.getUserMedia) {
          const constraints = { audio: true };
          navigator.mediaDevices.getUserMedia(constraints).then(
            stream => {
              console.log("授权成功！");
            },
            () => {
              console.error("授权失败！");
            }
          );
        } else {
            alert("浏览器不支持 getUserMedia");
        }
        this.flag3=!this.flag3;
        //当为true，暂停状态
        var timer=null;
        if(this.flag3){
          this.upTime3=10;
          try {
            this.recorder3.start();
          } catch (error) {
            alert("您的浏览器不支持");
            return;
          }
          this.isWork3=true;
          this.buttonMsg3="结束识别";
          timer=setInterval(()=>{
          if(this.upTime3==0){
              this.buttonMsg3="开始录音";
              this.flag3=false;
              clearInterval(timer)
              this.recorder3.stop();
              this.isWork3=false;
            }else{
              this.upTime3--;
              this.upTime3="0"+this.upTime3;
              if(this.flag3){
                this.buttonMsg3="结束识别";
              }else{
                this.isWork3=false;
                this.recorder3.stop();
                clearInterval(timer)
                this.buttonMsg3="开始录音";
              }
            }
          },1000)
        }else{
          this.buttonMsg3="开始录音";
          if(timer){
              clearInterval(timer)
          }
        }        
      },
      sendRecord2(val){
        let wav=this.recorder3.getWAVBlob();
        console.log(wav)
        let formData = new FormData(); //创建form对象
        formData.append('files', wav);//
        this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/voice/evaluation?text="+val+"&language="+this.source,
          data: formData,
        }).then(res => {
          if(res.data.code == 200000){
            if(!res.data.data){
              this.$Message.error('返回数据为空');
              return;
            }
            if(this.source=='en_us'){
              this.result=res.data.data.data.read_sentence.rec_paper.read_chapter.total_score;
            }else{
              this.result=res.data.data.data.read_sentence.rec_paper.read_sentence.total_score;
            }
          }
          else{
            this.$Message.error('请求失败');
          }
        }).catch(err=>{
          console.log(err);
        });
      },
      startRecord1(){
        let _this=this;
        var canRecording=ZCRecorder.canRecording;
        if(!canRecording){
          alert("不支持录音")
          return;
        }
          ZCRecorder.get(function(res){
            _this.recorder1=res;
            console.log(_this.recorder1)
            _this.flag=!_this.flag;
            //当为true，暂停状态
            var timer=null;
            if(_this.flag){
               _this.recorder1.start();
              _this.upTime=10;
              _this.numTime=0;
              _this.isWork=true;
              _this.buttonMsg="结束识别";
              let _data=null;
              timer=setInterval(()=>{
              if(_this.upTime==0){
                  _this.buttonMsg="开始录音";
                  _this.flag=false;
                  clearInterval(timer)
                  _data=_this.recorder1.getBlob();
                  _this.sendRecord1(_data);
                  _this.isWork=false;
                }else{
                  _this.numTime++;
                  _this.upTime--;
                  _this.upTime="0"+_this.upTime;
                  _data=_this.recorder1.getBlob();
                  _this.sendRecord1(_data);
                  if(_this.flag){
                    _this.buttonMsg="结束识别";
                  }else{
                    _this.isWork=false;
                    clearInterval(timer)
                     _data=_this.recorder1.getBlob();
                     _this.recorder1.stop();
                    _this.sendRecord1(_data);
                    _this.buttonMsg="开始录音";
                  }
                }
              },2000)
            }else{
              _this.buttonMsg="开始录音";
              if(timer){
                  clearInterval(timer)
              }
            }        

        },function(error){
          console.log(error)
        });
       
      },
      sendRecord1(val){
        let formData = new FormData(); //创建form对象
        formData.append('files', val);//
        this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/voice/asr?audioFormat=pcm16k16bit",
          data: formData,
        }).then(res => {
          if(res.data.code == 200000){
            if(!res.data.data){
              this.$Message.error('返回数据为空');
              return;
            }
            this.transMsg=res.data.data.ResponseInfo.Result.Text;
            //console.log(this.transMsg)
          }
          else{
            this.$Message.error('请求失败');
          }
        }).catch(err=>{
          console.log(err);
        });
      },
      startRecord(){
        if (navigator.mediaDevices.getUserMedia) {
          const constraints = { audio: true };
          navigator.mediaDevices.getUserMedia(constraints).then(
            stream => {
              console.log("授权成功！");
            },
            () => {
              console.error("授权失败！");
            }
          );
        } else {
            alert("浏览器不支持 getUserMedia");
        }
        this.flag=!this.flag;
        //当为true，暂停状态
        var timer=null;
        if(this.flag){
          this.upTime=10;
          try {
            this.recorder.start();
          } catch (error) {
            alert("您的浏览器不支持");
            return;
          }
          this.isWork=true;
          this.buttonMsg="结束识别";
          timer=setInterval(()=>{
          if(this.upTime==0){
              this.buttonMsg="开始录音";
              this.flag=false;
              clearInterval(timer)
              this.sendRecord();
              this.isWork=false;
            }else{
              this.upTime--;
              this.upTime="0"+this.upTime;
              if(this.flag){
                this.buttonMsg="结束识别";
              }else{
                this.isWork=false;
                this.sendRecord();
                clearInterval(timer)
                this.buttonMsg="开始录音";
              }
            }
          },1000)
        }else{
          this.buttonMsg="开始录音";
          if(timer){
              clearInterval(timer)
          }
        }        
      },
      sendRecord(val){
        this.recorder.stop();
        let wav=this.recorder.getWAVBlob();
        console.log(wav)
        let formData = new FormData(); //创建form对象
        formData.append('files', wav);//
        if(this.capkey){
          formData.append('capkey', this.capkey)
        }
        this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/voice/asr?audioFormat=pcm16k16bit",
          data: formData,
        }).then(res => {
          if(res.data.code == 200000){
            if(!res.data.data){
              this.$Message.error('返回数据为空');
              return;
            }
            this.transMsg=res.data.data.ResponseInfo.Result.Text;
          }
          else{
            this.$Message.error('请求失败');
          }
        }).catch(err=>{
          console.log(err);
        });
      },
    }
  }
</script>

<style lang='less'>
 .ivu-upload-drag{
  height: 100%;
  background: rgba(192,204,218,0.10);
  border: 1px solid #EBECF0;
}
.ivu-upload-drag:hover {
     border: 1px solid #EBECF0;
}
</style>
<style scoped lang='less'>

  .experience {
    display: flex;
    width: 100%;
    .fn_container{
      width: 100%;
      margin: auto;
      p{
        font-size: 14px;
        color: #7A8499;
        text-align: justify;
        line-height: 24px;
        margin-top: 10px;
      }
      .content{
         width: 100%;
         display: flex;
         justify-content: space-between;
        .title{
          font-size: 18px;
          color: #121C33;
          margin-bottom: 20px;
        }
        .left{
          flex:1;
          .left_box{
            height: 290px;
            border: 1px solid #EBECF0;
            .left_up{
              width: 100%;height:100%;background: rgba(192,204,218,0.10);
              display: flex;
              justify-content: center;
              align-items: center;
              flex-direction: column;
              .pic{
                width: 56px;
                img{
                  width: 100%;
                  display: block;
                }
              }
              .intro{
                font-size: 16px;
                color: #03002F;
                letter-spacing: 0;
                line-height: 14px;
                margin-top:30px;
                span{
                  color:#03a971;
                }
              }
            }
          }
        }
        .right{
           flex:1;
           margin-left:20px;
          .right_box{
            padding:10px;
            height: 290px;
            background: rgba(192,204,218,0.10);
            border: 1px solid #EBECF0;
          }
        }
      }
     
      
    }
    .duanyuyin_experience_container{
      width: 100%;
     .info_text_type{
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-bottom: 20px;
        .info_text{
          flex-grow: 1;
          font-size: 18px;
          color: #121C33;
        }
        .type{
          width: 18%;
        }
      }
      .type{
        width: 18%;
        margin-bottom: 20px;
      }
      .top{
        position: relative;
        width: 100%;
        height: 180px;
        margin-bottom: 10px;
        background: url(../../../assets/images/aiSmartAppDetail/tiyan.png) no-repeat center center;
        .icon{
          display:flex;justify-content:center;align-items:center;padding-top:50px;
          margin-bottom: 20px;
        }
        .times{
          text-align: center
        }
        img{
          width: 100%;
          height: 100%;
          display: block;
        }
        
      }
      .bottom{
        width: 100%;
        height: 180px;
        background: rgba(192,204,218,0.10);
        border: 1px solid #EBECF0;
        padding: 10px;
        font-size: 14px;
        color: #7A8499;
        text-align: justify;
        line-height: 22px;
      }
    }
  }
</style>
