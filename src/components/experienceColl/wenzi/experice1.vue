<template>
  <div class="experience">
      <Spin fix v-if="isloading">
          <Icon type="ios-loading" size=76 class="demo-spin-icon-load"></Icon>
          <div>加载中...</div>
      </Spin>
      <Modal v-model="visible" :footer-hide="true"  @on-cancel="cancle" class-name="vertical-center" width="1200" height="90%">
        <div class="iframe-box" id="iframe-box">
          <iframe :src="pdfUrl" frameborder="none" width="100%" height="100%" class="iframe" scrolling="yes" :style="{border:'none'}"></iframe>
        </div>
     </Modal>
      <!-- 已完成的功能 -->

      <!-- 营业执照识别 -->
    <div class="fn_container" v-if="routeId=='2.1.6'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                type="drag"
                :show-upload-list="false"
                :before-upload="handleBeforeUpload"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg">
          </div>
        </div>
      </div>
    </div>
      <!-- 身份证识别 -->
    <div class="fn_container" v-if="routeId=='2.1.1'">
      <RadioGroup v-model="templateType" class="radio_box" @on-change='changeCardSide'>
            <Radio label="front">身份证正面</Radio>
            <Radio label="back">身份证反面</Radio>
      </RadioGroup>
      <div class="content">
        
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                type="drag"
                :before-upload="handleBeforeUpload1"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                    <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic1" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg1">
           
          </div>
        </div>
      </div>
    </div>
      <!-- 机动车销售发票识别 -->
    <div class="fn_container" v-if="routeId=='2.3.6'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                type="drag"
                :show-upload-list="false"
                :before-upload="handleBeforeUpload2"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic2" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg2">
          </div>
        </div>
      </div>
    </div>
      <!-- 驾驶证识别 -->
    <div class="fn_container" v-if="routeId=='2.3.1'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                type="drag"
                 :show-upload-list="false"
                :before-upload="handleBeforeUpload3"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic3" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg3">
          </div>
        </div>
      </div>
    </div>
    <!-- 增值税发票识别 -->
    <div class="fn_container" v-if="routeId=='2.2.3'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload4"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic4" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg4">
          </div>
        </div>
      </div>
    </div>
    <!-- 教育场景文字识别 -->
    <!-- 手写文字识别 -->
    <div class="fn_container" v-if="routeId=='2.4.1'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload13"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic13" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg13">
          </div>
        </div>
      </div>
    </div>

    <!-- 公式识别 -->
    <div class="fn_container" v-if="routeId=='2.4.2'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload14"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic14" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg14">
          </div>
        </div>
      </div>
    </div>

    <!-- 其他文字识别 -->
    <!-- 表格文字识别 -->
    <div class="fn_container" v-if="routeId=='2.5.1'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload51"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic51" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="table">
            <div class="table_title_container">
              <div class="table_title" v-for="(item,index) in tableTitleArr" :key="index">{{item}}</div>
            </div>
            <div class="table_result" v-for="(item,index) in biaogeResultArr" :key="index">
              <div class="biaogeResult">{{item.row}}</div>
              <div class="biaogeResult">{{item.column}}</div>
              <div class="biaogeResult">{{item.word}}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- 数字识别 -->
    <div class="fn_container" v-if="routeId=='2.5.2'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload52"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic52" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg52">
          </div>
        </div>
      </div>
    </div>
    <!-- 印章检测 -->
    <div class="fn_container" v-if="routeId=='2.5.4'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload53"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic53" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg53">
          </div>
        </div>
      </div>
    </div>
    <!-- 网络图片文字识别 -->
    <div class="fn_container" v-if="routeId=='2.5.5'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload54"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic54" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg54">
          </div>
        </div>
      </div>
    </div>
    <!-- 二维码识别 -->
    <div class="fn_container" v-if="routeId=='2.5.3'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload55"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic55" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg55">
          </div>
        </div>
      </div>
    </div>
    <!-- 通用文字识别 -->
    <div class="fn_container" v-if="routeId=='2.6.1'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload61"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic61" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg61">
          </div>
        </div>
      </div>
    </div>
    <!-- 高精度版 -->
    <div class="fn_container" v-if="routeId=='2.6.2'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload62"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic62" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg62">
          </div>
        </div>
      </div>
    </div>
    <!-- 含位置信息版 -->
    <div class="fn_container" v-if="routeId=='2.6.3'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload63"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic63" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg63">
          </div>
        </div>
      </div>
    </div>
    <!-- 高精度含位置信息版 -->
    <div class="fn_container" v-if="routeId=='2.6.4'">
      <div class="content">
        <div class="left">
          <div class="title">上传图片：</div>
          <div class="left_box">
            <Upload
                ref="upload"
                 :show-upload-list="false"
                type="drag"
                :before-upload="handleBeforeUpload64"
                :max-size="5120"
                style="width:100%;height:100%"
                action=""
                :format="['jpg','jpeg','png','bmp']" 
                accept="image/png,image/jpeg,image/jpg,image/bmp"
                >
                <div class="left_up">
                   <div class="pic" :class="{ whFlag: !whFlag }">
                       <img :src="uploadPic64" alt="">
                    </div>
                    <div class="intro">
                      <div class="text">图片文件类型支持PNG、JPG、JPEG、BMP，图片大小不超过5M</div>
                      <i-button type="success">上传图片</i-button>
                    </div>
                </div>
            </Upload>
          </div>
        </div>
        <div class="right">
          <div class="title">识别结果：</div>
          <div class="right_box" v-html="msg64">
          </div>
        </div>
      </div>
    </div>
    <!-- 合并pdf文件 -->
    <div class="fn_container" v-if="routeId=='2.7.3'">
      <div class="content">
        <div class="left" style="height:480px">
          <div class="left_box" style="height:100%">
                <div class="left_up">
                    <div class="imgBox" v-for="(item,index) in fileList" :key="index">
                      <div class="imgPic" @mouseover="remove(item,index)">
                          <img src="../../../assets/images/pdf/pdf3.png" alt="">
                          <div class="del" v-if="index==currentIndex" @click="remove1(item,index)">
                            <img src="../../../assets/images/pdf/del.png" alt="">
                          </div>
                       </div>
                       <p>{{item.name}}</p>
                    </div>
                    <div class="intro">
                      <div class="text">目前只支持两个文件合并,文件大小不超过10M</div>
                      <div>
                          <Button type="success" style="margin-right:10px" @click="mergePdf">开始合并</Button>
                           <Upload
                            ref="upload"
                            multiple
                            :format="['pdf']"
                             accept=".pdf"
                            :show-upload-list="false"
                            :before-upload="handleBeforeUpload73"
                            :max-size="5120"
                            style="width:100%;height:100%;display:inline"
                            action=""
                            >
                              <Button type="success">上传pdf</Button >
                          </Upload>
                      </div>
                    </div>
                </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  import { imgPreviewBase64 , getImgSize} from '@/assets/js/imgPreviewBase64';
  export default {
     props:['routeId'],
     data() {
      return {
        whFlag:true,
        isloading:false,
        delShow:false,
        fileList:[],
        templateType:'back',
        isWork:false,
        upTime:10,
        numTime:0,
        transMsg:"请说出你想说的话...",
        buttonMsg:"开始录音",
        flag:false,
        recorder:null,
        recorder1:null,
        anyMsg:"",
        loading:false,  
        uploadPic64:require("@/assets/images/commonText/4.png"),
        msg64:``,
        uploadPic63:require("@/assets/images/commonText/3.png"),
        msg63:``,
        uploadPic62:require("@/assets/images/commonText/2.png"),
        msg62:``,
        uploadPic61:require("@/assets/images/commonText/tongY.png"),
        msg61:``,
        uploadPic55:require("@/assets/images/other/5.png"),
        msg55:``,
        uploadPic54:require("@/assets/images/other/4.png"),
        msg54:``,
        uploadPic53:require("@/assets/images/other/3.png"),
        msg53:``,
        uploadPic52:require("@/assets/images/other/2.png"),
        msg52:``,
        uploadPic51:require("@/assets/images/other/123.jpg"),
        msg51:``,
        uploadPic14:require("@/assets/images/education/2.png"),
        msg14:``,
        uploadPic13:require("@/assets/images/education/1.png"),
        msg13:``,
        uploadPic4:require("@/assets/images/carSence/fapiao.png"),
        msg4:``,
        uploadPic3:require("@/assets/images/carSence/2.png"),
        msg3:``,
        uploadPic2:require("@/assets/images/carSence/41.png"),
        msg2:``,
        uploadPic1:require("@/assets/images/card/shen1.png"),
        msg1:``,
        uploadPic:require("@/assets/images/card/12DA9F7A1DD38F3.png"),
        msg:``,
        currentIndex:null,
        pdfUrl:'',
        visible:false,
        tableTitleArr:['行数','列数','识别结果'],
        str:'{"form_num":1,"forms":[{"footer":[],"header":[{"rect":{"top":1081,"left":330,"width":889,"height":192},"column":[1],"row":[1],"word":"营养成分表"}],"body":[{"rect":{"height":152,"left":213,"top":562,"width":755},"column":[1],"row":[1],"word":"项目"},{"rect":{"height":153,"left":213,"top":714,"width":755},"column":[1],"row":[2],"word":"能量"},{"rect":{"height":152,"left":213,"top":867,"width":755},"column":[1],"row":[3],"word":"蛋白质"},{"rect":{"height":152,"left":213,"top":1019,"width":755},"column":[1],"row":[4],"word":"脂肪"},{"rect":{"height":153,"left":213,"top":1171,"width":755},"column":[1],"row":[5],"word":"碳水化合物"},{"rect":{"height":135,"left":213,"top":1324,"width":755},"column":[1],"row":[6],"word":"钠"},{"rect":{"height":152,"left":968,"top":562,"width":1006},"column":[2],"row":[1],"word":"每100克(g)"},{"rect":{"height":153,"left":968,"top":714,"width":1006},"column":[2],"row":[2],"word":"1367千焦(KJ)"},{"rect":{"height":152,"left":968,"top":867,"width":1006},"column":[2],"row":[3],"word":"23.4克(g)"},{"rect":{"height":152,"left":968,"top":1019,"width":1006},"column":[2],"row":[4],"word":"26.2克(g)"},{"rect":{"height":153,"left":968,"top":1171,"width":1006},"column":[2],"row":[5],"word":"0克(g)"},{"rect":{"height":135,"left":968,"top":1324,"width":1006},"column":[2],"row":[6],"word":"1124毫克(mg)"},{"rect":{"height":152,"left":1974,"top":562,"width":897},"column":[3],"row":[1],"word":"营养素参考值"},{"rect":{"height":153,"left":1974,"top":714,"width":897},"column":[3],"row":[2],"word":"16%"},{"rect":{"height":152,"left":1974,"top":867,"width":897},"column":[3],"row":[3],"word":"39%"},{"rect":{"height":152,"left":1974,"top":1019,"width":897},"column":[3],"row":[4],"word":"44%"},{"rect":{"height":153,"left":1974,"top":1171,"width":897},"column":[3],"row":[5],"word":"0%"},{"rect":{"height":135,"left":1974,"top":1324,"width":897},"column":[3],"row":[6],"word":"56%"}]}]}',
        biaogeResultArr:[],
      }
    },
    created() {
      let html=JSON.parse(this.str).forms[0].body;
      var result51 = [];
      for(var i = 0;i < html.length;i++){
        result51.push({
          row:(html[i].row + '').replace(/,/g,'-'),
          column:(html[i].column + '').replace(/,/g,'-'),
          word:html[i].word
        });
      }
      this.biaogeResultArr=result51;
    },
    mounted(){
      let _this=this;
      $(document).ready(function(){
        _this.msg64=`<table><tr><td>序号</td><td>内容</td></tr><tr><td>1</td><td><p> comIX齐心</p><p>宽度：240 高度：36</p><p>左间距：532 上间距：333</p></td></tr><tr><td>2</td><td><p>耐用铁网系列</p><p>宽度：301 高度：78</p><p>左间距：527 上间距：395</p></td></tr><tr><td>3</td><td><p>B2002圆笔筒</p><p>宽度：153 高度：25</p><p>左间距：607 上间距：485</p></td></tr></table>`;
        _this.msg63=`<table><tr><td>序号</td><td>内容</td></tr><tr><td>1</td><td><p>用安靜</p><p>宽度：400 高度：227</p><p>左间距：312 上间距：94</p></td></tr><tr><td>2</td><td><p>改變世界</p><p>宽度：508 高度：246</p><p>左间距：353 上间距：252</p></td></tr><tr><td>3</td><td><p>Quiet Influence</p><p>宽度：619 高度：235</p><p>左间距：412 上间距：396</p></td></tr><tr><td>4</td><td><p>The Introvert's Guide to Making a Difference</p><p>宽度：778 高度：220</p><p>左间距：428 上间距：481</p></td></tr></table>`;
        _this.msg62=`<table><tr><td>序号</td><td>内容</td></tr><tr><td>1</td><td><p>保罗·兰德</p></td></tr><tr><td>2</td><td><p>20世纪杰出的平面设计师、</p></td></tr><tr><td>3</td><td><p>思想家及设计教育家</p></td></tr><tr><td>4</td><td><p>令乔布斯折服的大师;被誉为“平面设计界的毕加索”</p></td></tr><tr><td>5</td><td><p>他设计的NEH、ABC、BM、UPS等商标;蔚为经典</p></td></tr><tr><td>6</td><td><p>PAUL RAND</p></td></tr><tr><td>7</td><td><p>设计的意义</p></td></tr><tr><td>8</td><td><p>保罗·兰德谈设计、形式与混沌</p></td></tr></table>`;
        _this.msg61=`<table><tr><td>序号</td><td>内容</td></tr><tr><td>1</td><td><p>第一部分教材知识梳理</p></td></tr><tr><td>2</td><td><p>模块三中国现代史</p></td></tr><tr><td>3</td><td><p>第一单元</p></td></tr><tr><td>4</td><td><p>中华人民共和国的成立和巩固</p></td></tr></table>`;
        _this.msg55=`<table><tr><td>序号</td><td>内容</td></tr><tr><td>1</td><td><p>类型：QR_CODE</p><p>内容：["hello world"]</p></td></tr></table>`;
        _this.msg54=`<table><tr><td>序号</td><td>内容</td></tr><tr><td>1</td><td><p>梦想起航</p></td></tr><tr><td>2</td><td><p>前往下一个目的地</p></td></tr><tr><td>3</td><td><p>也</p></td></tr><tr><td>4</td><td><p>开始新的旅程</p></td></tr></table>`;
        _this.msg53=`<table><tr><td>序号</td><td>内容</td></tr><tr><td>1</td><td><p>宽度：241</p><p>高度：172</p><p>上间距：136</p><p>左间距：506</p></td></tr><tr><td>2</td><td><p>宽度：340</p><p>高度：245</p><p>上间距：414</p><p>左间距：728</p></td></tr></table>`;
        _this.msg52=`<table><tr><td>序号</td><td>内容</td></tr><tr><td>1</td><td><p>543543777555638</p><p>宽度：242 高度：23</p><p>左间距：409 上间距：255</p></td></tr><tr><td>2</td><td><p>17600987577</p><p>宽度：201 高度：35</p><p>左间距：714 上间距：348</p></td></tr><tr><td>3</td><td><p>15798448856</p><p>宽度：192 高度：28</p><p>左间距：721 上间距：536</p></td></tr></table>`;
        _this.msg51=`中华人民共和国机动车行驶证 Vehicle License of the People's Republic of China号牌号码鲁A8 Plate No车辆类型小型轿车 Vehicle Type所有人济南万右汽有限 Owner住址山东省济南市视区士 Addiess室使用性质非营运品牌型号宝马牌 BMWTESL Use Characte Model山东省济南车辆识别代号LBV8W VIN市公安局交发动机号码A355`;
        _this.msg14=`36x79x96`;
        _this.msg13=`<p>2019.5.23会议纪要</p><p>一、项目进度汇报</p><p>二、首页改版头脑风暴</p><p>三、本周遇到的问题</p>`;
        _this.msg4=`<p>价税合计(大写)：陆仟壹佰叁拾贰圆伍角整</p><p>开票人：焦红娟</p><p>销售方地址及电话：北京市朝阳区64377727</p><p>数量：</p><p>销售方纳税人识别号：110105057317113</p><p>销售方开户行及账号：上海浦发银行91150154740007408</p><p>备注：2016.9</p><p>税率：6%</p><p>合计税额：347.12</p><p>发票代码：1100154130</p><p>开票日期：2016年11月15日</p><p>购方纳税人识别号：110108802100433</p><p>发票名称：北京增值税专用发票</p><p>购方开户行及账号：招商银行北京分行上地支行110902160610706</p><p>价税合计(小写)：6132.50</p><p>复核：马学琦</p><p>合计金额：5785.38</p><p>金额：5785.38</p><p>购方名称：北京百度网讯科技有限公司</p><p>发票种类：专用发票</p><p>购方地址及电话：北京市海淀区上地十街10号百度大厦2层010-59928888</p><p>税额：347.12</p><p>销售方名称：北京圣紫茗管理咨询有限公司</p><p>货物名称：服务费</p><p>发票号码：00772445</p>`;
        _this.msg3=`<p>姓名：陈涛</p><p>至：20221002</p><p>证号：370481198711989111</p><p>出生日期：19891111</p><p>住址：山东省滕州市龙阳镇</p><p>国籍：中国</p><p>初次领证日期：20161002</p><p>准驾车型：C1</p><p>有效期限：20161002</p><p>性别：男</p>`;
        _this.msg2=`<p>产地：山东省青岛莱西市</p><p>厂牌型号：北京牌BJ7001BPH5-BEV</p><p>税额：￥8252.99</p><p>主管税务机关代码：11101057605</p><p>限乘人数	：</p><p>购买方身份证号码/组织机构代码：</p><p>销货单位名称：北京庞大华成昌业汽车销售服务有限</p><p>机器编号：499911939399</p><p>合格证号：YP45X1000083349</p><p>税率：17%</p><p>购买方名称：</p><p>发票代码/机打代码：111001622011</p><p>开票日期：2018-01-22</p><p>销货单位地址：北京市朝阳区王四营乡道口村村东</p><p>销货单位纳税人识别号：91110105567481087R</p><p>销货单位开户银行：交通银行北京东区支行</p><p>车辆类型：纯电动轿车</p><p>车架号码：</p><p>价税合计：南万陆仔损佰圆整</p><p>销货单位电话：010-67207759</p><p>主管税务机关：北京市朝阳区国家税务</p><p>不含税价格：</p><p>销货单位账号：110061166018010157859</p><p>发动机号码：</p><p>发票号码/机打号码：</p><p>价税合计小写：￥56800.00</p>`;
        _this.msg1=`<p>失效日期：20390102</p><p>签发机关：天津市公安局和平分局</p><p>签发日期：20190102</p>`;
        _this.msg=`<p>社会信用代码：997449337210928</p><p>组成形式：无</p><p>经营范围：无</p><p>法人：李双双</p><p>成立日期：无</p><p>注册资本：无</p><p>证件编号：330200334928321</p><p>地址：四川省成都市新林大路121</p><p>单位名称：成都市华莲商贸有限公司</p><p>类型：无</p><p>有效期：2050年08月21日</p>`;
      })
    },
    methods:{
      changeCardSide(e){
        if(e == 'front'){
          var img1 = require('../../../assets/images/card/1.png');
          this.uploadPic1 = img1;
          this.msg1 = `
            <p>姓名：王飞</p><p>民族：汉</p><p>住址：辽宁省大连市甘井子区</p><p>公民身份号码：522530199208180048</p><p>出生：19920818</p><p>性别：女</p>
          `;
        }  
        else if(e == 'back'){
          var img2 = require('../../../assets/images/card/shen1.png');
          this.uploadPic1 = img2;
          this.msg1 = `
           <p>失效日期：20390102</p><p>签发机关：天津市公安局和平分局</p><p>签发日期：20190102</p>
          `;
        }  
      },
      cancle(){
        this.visible=false;
      },
      remove1(item,index){
        this.fileList.splice(index,1);
      },
      remove(item,index){
        this.currentIndex=index;
      },
      mergePdf(){
        if(this.fileList.length!=2){
          this.$Message.warning('目前只支持两个文件合并！')
          return
        }
         let formData = new FormData(); //创建form对象
         formData.append('file',this.fileList[0]);
         formData.append('file2', this.fileList[1]);
          this.isloading=true;
          this.$axios({
            headers: {
                'Accept': '*/*',
                'Content-Type': 'application/x-www-form-urlencoded'
            },
            method: "post",
            url: window.config.url+"/api/pdf/merge",
            data: formData,
          }).then(res => {
               this.isloading=false;
            if(res.data.code == 200){
              if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
              }
              this.visible=true;
              this.pdfUrl=res.data.data;
            }
            else{
              this.$Message.error('请求失败');
            }
          }).catch(err=>{
             this.isloading=false;
             this.isloading=false;
            console.log(err);
          });
      },
      handleBeforeUpload73(file){
        this.fileList.push(file);
      },
      handleBeforeUpload64(file){
        var _this = this;
        imgPreviewBase64(_this, file, function(base64){
          _this.uploadPic64=base64;
          getImgSize(_this.uploadPic64).then(res => {
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
          url: window.config.url+"/ai/text/accurate",
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            if(res.data.data){ 
              let html=`<table><tr><td>序号</td><td>内容</td></tr>`;
              res.data.data.words_result.forEach((item,index)=>{
                if(item.words){
                  html+=`<tr><td>${index+1}</td><td><p>${item.words}</p><p>宽度：${item.location.width} 高度：${item.location.height}</p><p>左间距：${item.location.left} 上间距：${item.location.top}</p></td></tr>`
                }
              })
              this.msg64=html+`</table>`;
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
      handleBeforeUpload63(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic63=base64;
            getImgSize(_this.uploadPic63).then(res => {
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
              let html=`<table><tr><td>序号</td><td>内容</td></tr>`;
              res.data.data.words_result.forEach((item,index)=>{
                if(item.words){
                  html+=`<tr><td>${index+1}</td><td><p>${item.words}</p><p>宽度：${item.location.width} 高度：${item.location.height}</p><p>左间距：${item.location.left} 上间距：${item.location.top}</p></td></tr>`
                }
              })
              this.msg63=html+`</table>`;
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
      handleBeforeUpload62(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic62=base64;
            getImgSize(_this.uploadPic62).then(res => {
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
          url: window.config.url+"/ai/text/accurate",
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            if(res.data.data){
              let html=`<table><tr><td>序号</td><td>内容</td></tr>`;
              res.data.data.words_result.forEach((item,index)=>{
                if(item.words){
                  html+=`<tr><td>${index+1}</td><td><p>${item.words}</p></td></tr>`
                }
              })
              this.msg62=html+`</table>`;
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
      handleBeforeUpload61(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic61=base64;
            getImgSize(_this.uploadPic61).then(res => {
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
              let html=`<table><tr><td>序号</td><td>内容</td></tr>`;
              res.data.data.words_result.forEach((item,index)=>{
                if(item.words){
                  html+=`<tr><td>${index+1}</td><td><p>${item.words}</p></td></tr>`
                }
              })
              this.msg61=html+`</table>`;
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
      handleBeforeUpload55(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic55=base64;
            getImgSize(_this.uploadPic55).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "post",
          url: window.config.url+"/ai/text/qrcode",
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            if(res.data.data){
              let html=`<table><tr><td>序号</td><td>内容</td></tr>`;
              res.data.data.codes_result.forEach((item,index)=>{
                if(item.text){
                  html+=`<tr><td>${index+1}</td><td><p>类型：${item.type}</p><p>内容：["${item.text}"]</p></td></tr>`
                }
              })
              this.msg55=html+`</table>`;
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
      handleBeforeUpload54(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic54=base64;
             getImgSize(_this.uploadPic54).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "post",
          url: window.config.url+"/ai/text/webImage",
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            if(res.data.data){
              let html=`<table><tr><td>序号</td><td>内容</td></tr>`;
              res.data.data.words_result.forEach((item,index)=>{
                if(item.words){
                  html+=`<tr><td>${index+1}</td><td><p>${item.words}</p></td></tr>`
                }
              })
              this.msg54=html+`</table>`;
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
      handleBeforeUpload53(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic53=base64;
            getImgSize(_this.uploadPic53).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "post",
          url: window.config.url+"/ai/text/seal",
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            if(res.data.data){
              let html=`<table><tr><td>序号</td><td>内容</td></tr>`;
              res.data.data.result.forEach((item,index)=>{
                if(item.location){
                  html+=`<tr><td>${index+1}</td><td><p>宽度：${item.location.width}</p><p>高度：${item.location.height}</p><p>上间距：${item.location.top}</p><p>左间距：${item.location.left}</p></td></tr>`
                }
              })
              this.msg53=html+`</table>`;
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
      handleBeforeUpload52(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic52=base64;
            getImgSize(_this.uploadPic52).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "post",
          url: window.config.url+"/ai/text/numbers",
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
              this.$Message.error('返回数据为空');
              return;
            }
            if(res.data.data){
              let html=`<table><tr><td>序号</td><td>内容</td></tr>`;
              res.data.data.words_result.forEach((item,index)=>{
                if(item.words){
                  html+=`<tr><td>${index+1}</td><td><p>${item.words}</p><p>宽度：${item.location.width} 高度：${item.location.height}</p><p>左间距：${item.location.left} 上间距：${item.location.top}</p></td></tr>`
                }
              })
              this.msg52=html+`</table>`;
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
      handleBeforeUpload51(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic51=base64;
            getImgSize(_this.uploadPic51).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         formData.append('requestType','json');
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "post",
          url: window.config.url+"/ai/text/formOcrRequest",
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            if(res.data.data){
              this.biaogeResultArr= [];
              let result=JSON.parse(res.data.data.result.result_data);
              let html=result.forms[0].body;
              var result51 = [];
              for(var i = 0;i < html.length;i++){
                result51.push({
                  row:(html[i].row + '').replace(/,/g,'-'),
                  column:(html[i].column + '').replace(/,/g,'-'),
                  word:html[i].word
                });
              }
              this.biaogeResultArr=result51;
            }else{
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
      handleBeforeUpload14(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic14=base64;
            getImgSize(_this.uploadPic14).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "post",
          url: window.config.url+"/ai/text/formula",
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
              let html=``;
              result.forEach(item=>{
                if(item.words){
                  html+=`<p>${item.words}</p>`
                }
              })
              this.msg14=html;
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
      handleBeforeUpload13(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic13=base64;
            getImgSize(_this.uploadPic13).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: "post",
          url: window.config.url+"/ai/text/handwriting",
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
              let html=``;
              result.forEach(item=>{
                if(item.words){
                  html+=`<p>${item.words}</p>`
                }
              })
              this.msg13=html;
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
      handleBeforeUpload4(file){
        console.log(file)
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic4=base64;
            getImgSize(_this.uploadPic4).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/bdocr/vatInvoice?img_type=FILES",
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
                let html=``;
                for(var key in result){
                  if(key=='AmountInWords'){
                      html+=`<p>价税合计(大写)：${result[key]}</p>`;
                  }
                  else if(key=='NoteDrawer'){
                     html+=`<p>开票人：${result[key]}</p>`;
                  }
                  else if(key=='SellerAddress'){
                     html+=`<p>销售方地址及电话：${result[key]}</p>`;
                  }
                  else if(key=='CommodityNum'){
                     html+=`<p>数量：${result[key][0].word}</p>`;
                  }
                  else if(key=='SellerRegisterNum'){
                     html+=`<p>销售方纳税人识别号：${result[key]}</p>`;
                  }
                  else if(key=='SellerBank'){
                     html+=`<p>销售方开户行及账号：${result[key]}</p>`;
                  }
                  else if(key=='Remarks'){
                     html+=`<p>备注：${result[key]}</p>`;
                  }
                  else if(key=='CommodityTaxRate'){
                     html+=`<p>税率：${result[key][0].word}</p>`;
                  }
                  else if(key=='TotalTax'){
                     html+=`<p>合计税额：${result[key]}</p>`;
                  }
                  else if(key=='InvoiceCode'){
                     html+=`<p>发票代码：${result[key]}</p>`;
                  }
                  else if(key=='InvoiceDate'){
                     html+=`<p>开票日期：${result[key]}</p>`;
                  }
                  else if(key=='PurchaserRegisterNum'){
                     html+=`<p>购方纳税人识别号：${result[key]}</p>`;
                  }
                  else if(key=='InvoiceTypeOrg'){
                     html+=`<p>发票名称：${result[key]}</p>`;
                  }
                  else if(key=='PurchaserBank'){
                     html+=`<p>购方开户行及账号：${result[key]}</p>`;
                  }
                  else if(key=='AmountInFiguers'){
                     html+=`<p>价税合计(小写)：${result[key]}</p>`;
                  }
                  else if(key=='Checker'){
                     html+=`<p>复核：${result[key]}</p>`;
                  }
                  else if(key=='TotalAmount'){
                     html+=`<p>合计金额：${result[key]}</p>`;
                  }
                  else if(key=='CommodityAmount'){
                     html+=`<p>金额：${result[key][0].word}</p>`;
                  }
                  else if(key=='PurchaserName'){
                     html+=`<p>购方名称：${result[key]}</p>`;
                  }
                  else if(key=='InvoiceType'){
                     html+=`<p>发票种类：${result[key]}</p>`;
                  }
                  else if(key=='PurchaserAddress'){
                     html+=`<p>购方地址及电话：${result[key]}</p>`;
                  }
                  else if(key=='CommodityTax'){
                     html+=`<p>税额：${result[key][0].word}</p>`;
                  }
                  else if(key=='SellerName'){
                     html+=`<p>销售方名称：${result[key]}</p>`;
                  }
                  else if(key=='CommodityName'){
                     html+=`<p>货物名称：${result[key][0].word}</p>`;
                  }
                  else if(key=='InvoiceNum'){
                     html+=`<p>发票号码：${result[key]}</p>`;
                  }
                  // if(typeof(result[key])=='string'){
                  //     html+=`<p>${key}：${result[key]}</p>`;
                  // }else{
                  //   if(result[key].length>0){
                  //      html+=`<p>${key}：${result[key][0].word}</p>`;
                  //   }
                  // }
                }
              this.msg4=html;
              console.log(html)
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
      handleBeforeUpload3(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic3=base64;
            getImgSize(_this.uploadPic3).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/bdocr/drivingLicense?img_type=FILES",
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
                let html=``;
                for(var key in result){
                    html+=`<p>${key}：${result[key].words}</p>`;
                }
                this.msg3=html;
                
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
      handleBeforeUpload2(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic2=base64;
            getImgSize(_this.uploadPic2).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); //创建form对象
         formData.append('files', file);//
         this.isloading=true;
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/bdocr/vehicleInvoice?img_type=FILES",
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
                let html=``;
                for(var key in result){
                  if(key=='Origin'){
                      html+=`<p>产地：${result[key]}</p>`;
                  }
                  else if(key=='ManuModel'){
                     html+=`<p>厂牌型号：${result[key]}</p>`;
                  }
                  else if(key=='Tax'){
                     html+=`<p>税额：${result[key]}</p>`;
                  }
                  else if(key=='TaxAuthorCode'){
                     html+=`<p>主管税务机关代码：${result[key]}</p>`;
                  }
                  else if(key=='LimitPassenger'){
                     html+=`<p>限乘人数	：${result[key]}</p>`;
                  }
                  else if(key=='PurchaserCode'){
                     html+=`<p>购买方身份证号码/组织机构代码：${result[key]}</p>`;
                  }
                  else if(key=='Saler'){
                     html+=`<p>销货单位名称：${result[key]}</p>`;
                  }
                  else if(key=='MachineCode'){
                     html+=`<p>机器编号：${result[key]}</p>`;
                  }
                  else if(key=='CertificateNum'){
                     html+=`<p>合格证号：${result[key]}</p>`;
                  }
                  else if(key=='TaxRate'){
                     html+=`<p>税率：${result[key]}</p>`;
                  }
                  else if(key=='Purchaser'){
                     html+=`<p>购买方名称：${result[key]}</p>`;
                  }
                  else if(key=='InvoiceCode'){
                     html+=`<p>发票代码/机打代码：${result[key]}</p>`;
                  }
                  else if(key=='InvoiceDate'){
                     html+=`<p>开票日期：${result[key]}</p>`;
                  }
                  else if(key=='SalerAddress'){
                     html+=`<p>销货单位地址：${result[key]}</p>`;
                  }
                  else if(key=='SalerCode'){
                     html+=`<p>销货单位纳税人识别号：${result[key]}</p>`;
                  }
                  else if(key=='SalerBank'){
                     html+=`<p>销货单位开户银行：${result[key]}</p>`;
                  }
                  else if(key=='VehicleType'){
                     html+=`<p>车辆类型：${result[key]}</p>`;
                  }
                  else if(key=='VinNum'){
                     html+=`<p>车架号码：${result[key]}</p>`;
                  }
                  else if(key=='PriceTax'){
                     html+=`<p>价税合计：${result[key]}</p>`;
                  }
                  else if(key=='SalerPhone'){
                     html+=`<p>销货单位电话：${result[key]}</p>`;
                  }
                  else if(key=='TaxAuthor'){
                     html+=`<p>主管税务机关：${result[key]}</p>`;
                  }
                  else if(key=='Price'){
                     html+=`<p>不含税价格：${result[key]}</p>`;
                  }
                  else if(key=='SalerAccountNum'){
                     html+=`<p>销货单位账号：${result[key]}</p>`;
                  }
                  else if(key=='EngineNum'){
                     html+=`<p>发动机号码：${result[key]}</p>`;
                  }
                  else if(key=='InvoiceNum'){
                     html+=`<p>发票号码/机打号码：${result[key]}</p>`;
                  }
                  else if(key=='PriceTaxLow'){
                     html+=`<p>价税合计小写：${result[key]}</p>`;
                  }
                    // html+=`<p>${key}：${result[key]}</p>`;
                }
              this.msg2=html;
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
      handleBeforeUpload1(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic1=base64;
            getImgSize(_this.uploadPic1).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); //创建form对象
         formData.append('files', file);//
         this.isloading=true;
         this.msg1='';
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/bdocr/idcard?img_type=FILES&id_card_side="+this.templateType,
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
                this.$Message.error('返回数据为空');
                return;
            }
            let result=res.data.data.words_result;
            let html=``;
            for(var key in result){
                html+=`<p>${key}：${result[key].words}</p>`;
            }
            this.msg1=html;
          }
          else{
              this.$Message.error('请求失败');
          }
        }).catch(err=>{
           this.isloading=false;
          console.log(err);
        });
      },
      handleBeforeUpload(file){
          var _this = this;
          imgPreviewBase64(_this, file, function(base64){
            _this.uploadPic=base64;
            getImgSize(_this.uploadPic).then(res => {
              _this.whFlag = res.flag;
            });
          })
         let formData = new FormData(); 
         formData.append('files', file);
         this.isloading=true;
         this.msg='';
         this.$axios({
          headers: {
              'Accept': '*/*',
              'Content-Type': 'multipart/form-data'
          },
          method: "post",
          url: window.config.url+"/ai/bdocr/businessLicense?img_type=FILES",
          data: formData,
        }).then(res => {
         this.isloading=false;
         if(res.data.code == 200000){
            if(!res.data.data){
              this.$Message.error('返回数据为空');
              return;
            }
             let result=res.data.data.words_result;
            let html=``;
            for(var key in result){
                html+=`<p>${key}：${result[key].words}</p>`;
            }
            this.msg=html;
          }
          else{
              this.$Message.error('请求失败');
          }
        }).catch(err=>{
           this.isloading=false;
          console.log(err);
        });
      }
  }
}
</script>
<style lang='less'>

.vertical-center{
        display: flex;
        align-items: center;
        justify-content: center;
        .ivu-modal{
            top: 0;
        }
        .ivu-modal{
          height: 90%;
        }
        .ivu-modal-content{
          height: 100%;
        }
        .ivu-modal-body{
          height: 100%;
        }
    }
  table{
    border-spacing:15px 0px;
  }
  td{
  vertical-align:top;
  }
 .demo-spin-icon-load{
        animation: ani-demo-spin 1s linear infinite;
    }
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
 .iframe-box {
  width:100%;
  height: 100%;
}
  .experience {
    display: flex;
    width: 100%;
    position: relative;
    .ivu-spin-fix{
      background: none;
      // color:#03a971;
    }
    .fn_container{
      width: 100%;
      margin: auto;
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
            // background: rgba(192,204,218,0.10);
            border: 1px solid #EBECF0;
            .left_up{
              width: 100%;height:100%;background: rgba(192,204,218,0.10);
              position: relative;
              .imgBox{
                display: inline-block;
                margin: 20px 50px;
                .imgPic{
                   width:60px;
                   margin:auto;
                   position: relative;
                   img{
                      width: 100%;
                      display:block;
                    }
                    .del{
                      position: absolute;
                      right: -8px;
                      top:-3px;
                      width: 15px;
                      cursor: pointer;
                    }
                }
                
                p{
                  text-align: center;
                }
              }
              .pic{
                width: 100%;
                &.whFlag {
                  height: 100%;
                  display: flex;
                  align-items: center;
                  justify-content: center;
                  overflow: hidden;
                  img {
                    width: auto;
                    height: 100%;
                  }

                  canvas {
                    width: auto;
                    height: 100%;
                  }
                }
                img{
                  width: 100%;
                  display: block;
                }
              }
              .intro{
                display: flex;
                justify-content: space-between;
                padding:0 15px;
                align-items: center;
                opacity: 0.85;
                background: #121C33;
                position: absolute;
                bottom:0;
                left:0;
                width: 100%;
                height: 50px;
                .text{
                   font-size: 12px;
                  color: #FFFFFF;
                  text-align: justify;
                }
               
              }
            }
            .ivu-upload{
              /deep/.ivu-upload-select{
                .ivu-btn-default{
                  background: #03A971 !important;
                  border: none !important;
                  span{
                    color: #ffffff;
                  }
                } 
              }
              /deep/.ivu-upload-list{
                display: none;
              }
            }
          }
        }
        .right{
           flex:1;
           margin-left:20px;
          .right_box{
            overflow: auto;
            font-size: 14px;
            color: #121C33;
            letter-spacing: 0;
            line-height: 24px;
            padding:10px;
            height: 290px;
            background: rgba(192,204,218,0.10);
            border: 1px solid #EBECF0;
          }
          .table{
            font-size: 14px;
            color: #121C33;
            letter-spacing: 0;
            line-height: 24px;
            padding: 10px;
            height: 290px;
            background: rgba(192, 204, 218, 0.1);
            border: 1px solid #EBECF0;
            .table_title_container{
              display: flex;
              justify-content: space-around;
              .table_title{
                width: 32%;
                font-size: 14px;
                color: #121C33;
                text-align: center;
              }
            }
            .table_result{
              display: flex;
              justify-content: space-around;
              .biaogeResult{
                width: 32%;
                font-size: 12px;
                color: #121C33;
                text-align: center;
                margin-bottom: 5px;
              }
            }
          }
          .table{
            overflow: hidden !important;
          }
          .table:hover{
            overflow: auto !important;
          }
          .table::-webkit-scrollbar{
            width: 5px;     
            height: 5px;
          }
          .table::-webkit-scrollbar-thumb{/*滚动条里面小方块*/
            border-radius: 5px;
            background: #999999;
            height: 40px;
          }
          .table::-webkit-scrollbar-track
          {/*滚动条里面轨道*/
            border-radius: 0;
            background: white;
          }
        }
      }
    }
    .demo-spin-col{
      position: absolute;
      left: 50%;
      top: 52%;
      /deep/.ivu-spin-main{
        width: 140px;
      }
    }
  }
</style>
