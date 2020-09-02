<template>
<div class="resumeList_container">
    <div class="banner_container">
        <div class="banner_content">
            <div class="banner_title">AI推荐简历</div>
            <div class="banner_desc">致力于通过AI技术提高招聘速度，降低时间成本，节省招聘支出。</div>
            <div class="banner_btn" @click="goHezuo">合作咨询</div>
        </div>
    </div>
    <div class="choice_job_btn_container">
        <div class="tuijian_title">请选择要推荐的岗位：</div>
        <div class="job_type_item_container">
            <div class="job_type_item">
                <div class="left_img"></div>
                <div class="right_text_btn"></div>
            </div>
            <div class="job_type_item"></div>
            <div class="job_type_item"></div>
        </div>
    </div>
</div>
</template>

<script>
export default {
  data() {
    return {
        JobList: [
            {
                value: 'asd123',
                label: '算法工程师'
            },
            {
                value: 'asd124',
                label: '高级Java工程师'
            },
            {
                value: 'asd125',
                label: '行政助理'
            }
        ],
        jobSelectValue:'asd123',
        jobContent:`岗位职责：
                    <br>
                    1、协助研究新算法，改进现有的算法；
                    <br>
                    2、协助研究并实现摄像机图像的智能分析算法与ISP算法；
                    <br>
                    3、协助处理数据，需要进行一定量的信息检索、数据挖掘；
                    <br>
                    4、协助编写设计文档以及说明文档；
                    <br>
                    5、熟悉公司产品，使用算法来实现公司产品需求；
                    <br>
                    任职要求：
                    <br>
                    1、本科及以上学历，计算机、通信、电子、自动化等相关专业；
                    <br>
                    2、一年以上机器视觉与图像处理行业项目开发经验；
                    <br>
                    3、熟练掌握图像处理算法原理，精通OpenCV，熟悉Halcon等视觉算法库及工具；
                    <br>
                    4、精通C++/C#编程语言；
                    <br>
                    5、对图像识别算法有深刻理解及应用经验优先；
                    <br>
                    6、具有强的协调沟通能力、分析解决问题能力及团队合作精神；
                    <br>
                    7、熟悉神经网络、支持向量机、深度学习等优先考虑；
                    <br>
                    8、 熟悉主流的机器学习算法，能够熟练使用深度学习TensorFlow、Caffe等深度学习框架。`,
        resumeList:[],
        ifShowResult:false,
        avatarImg:require('../../assets/images/JobFile/avatar.png'),
        suanfaZhizeText:`岗位职责：
                        <br>
                        1、协助研究新算法，改进现有的算法；
                        <br>
                        2、协助研究并实现摄像机图像的智能分析算法与ISP算法；
                        <br>
                        3、协助处理数据，需要进行一定量的信息检索、数据挖掘；
                        <br>
                        4、协助编写设计文档以及说明文档；
                        <br>
                        5、熟悉公司产品，使用算法来实现公司产品需求；`,
        suanfaYaoqiuText:`任职要求：
                        <br>
                        1、本科及以上学历，计算机、通信、电子、自动化等相关专业；
                        <br>
                        2、一年以上机器视觉与图像处理行业项目开发经验；
                        <br>
                        3、熟练掌握图像处理算法原理，精通OpenCV，熟悉Halcon等视觉算法库及工具；
                        <br>
                        4、精通C++/C#编程语言；
                        <br>
                        5、对图像识别算法有深刻理解及应用经验优先；
                        <br>
                        6、具有强的协调沟通能力、分析解决问题能力及团队合作精神；
                        <br>
                        7、熟悉神经网络、支持向量机、深度学习等优先考虑；
                        <br>
                        8、 熟悉主流的机器学习算法，能够熟练使用深度学习TensorFlow、Caffe等深度学习框架。`,
        javaZhizeText:`岗位职责：
                        <br> 
                        1、负责软件开发、核心代码编写、修改bug等工作；
                        <br> 
                        2、负责攻克技术研发中的难点与关键点；
                        <br> 
                        3、参与产品构思和架构设计；
                        <br> 
                        4、编写技术代码及文件，并对开发代码做妥善管理
                        <br> 
                        5、完成上级领导交办的其他工作。
                        <br>`,
        javaYaoqiuText:`任职要求：
                        <br> 
                        1、计算机或相关专业本科以上学历，4年以上Java开发工作经验； 
                        <br>
                        2、精通Java语言，代码编写规范，编程基础扎实，逻辑思维能力强；
                        <br> 
                        3、对JVM，多线程有深入理解及实际经验； 
                        <br>
                        4、深入理解设计模式，熟悉常见的数据结构和算法； 
                        <br>
                        5、有分布式系统开发设计经验，熟悉spring、spring boot、等开源框架；
                        <br> 
                        6、熟悉mysql数据库，redis等开发与优化； 
                        <br>
                        7、良好的学习能力，有激情，能承受较大压力。`,
        xingzhengZhizeText:`岗位职责：
                            <br> 
                            1、负责前台电话的接听和转接；
                            <br>  
                            2、负责来访客户的接待工作；
                            <br>  
                            3、负责收发公司信件、快递，订水等； 
                            <br> 
                            4、负责办公环境的维护及管理； 
                            <br> 
                            5、负责办公用品的采购、发放、管理； 
                            <br> 
                            6、上级交办的其他行政类工作。
                            <br>`,
        xingzhengYaoqiuText:`任职要求：
                            <br> 
                            1、大专学历， 行政类、英语或医疗专业优先，一年以上行政类工作经验； 
                            <br>
                            2、普通话标准，年龄24-28岁，形象气质佳；
                            <br> 
                            3、善于沟通，性格外向，工作积极主动；
                            <br> 
                            4、熟练操作word、excel等办公软件；
                            <br>
                            5、有驾照并熟练驾驶以及英语听说读写优秀者优先`,
        zhizeText:`岗位职责：
                    <br>
                    1、协助研究新算法，改进现有的算法；
                    <br>
                    2、协助研究并实现摄像机图像的智能分析算法与ISP算法；
                    <br>
                    3、协助处理数据，需要进行一定量的信息检索、数据挖掘；
                    <br>
                    4、协助编写设计文档以及说明文档；
                    <br>
                    5、熟悉公司产品，使用算法来实现公司产品需求；`,
        yaoqiuText:`任职要求：
                    <br>
                    1、本科及以上学历，计算机、通信、电子、自动化等相关专业；
                    <br>
                    2、一年以上机器视觉与图像处理行业项目开发经验；
                    <br>
                    3、熟练掌握图像处理算法原理，精通OpenCV，熟悉Halcon等视觉算法库及工具；
                    <br>
                    4、精通C++/C#编程语言；
                    <br>
                    5、对图像识别算法有深刻理解及应用经验优先；
                    <br>
                    6、具有强的协调沟通能力、分析解决问题能力及团队合作精神；
                    <br>
                    7、熟悉神经网络、支持向量机、深度学习等优先考虑；
                    <br>
                    8、 熟悉主流的机器学习算法，能够熟练使用深度学习TensorFlow、Caffe等深度学习框架。`
    };
  },
  methods: {
      goHezuo(){
        this.$router.push({
            path:'/zixun'
        });
      }
  }
};
</script>
<style scoped lang='less'>
.resumeList_container{
    width: 100%;
    height: 100%;
    .banner_container{
        width: 100%;
        height: 350px;
        background-image: url('../../assets/images/JobFile/banner.png');
        background-size: cover;
        background-position: center;
        padding-top: 81px;
        box-sizing: border-box;
        display: flex;
        justify-content: center;
        .banner_content{
            width: 1200px;
            .banner_title{
                font-family: PingFangSC-Medium;
                font-size: 30px;
                color: #FFFFFF;
                margin-bottom: 15px;
            }
            .banner_desc{
                font-family: PingFangSC-Regular;
                font-size: 14px;
                color: #FFFFFF;
                line-height: 28px;
            }
            .banner_btn{
                margin-top: 54px;
                background: #03A971;
                border-radius: 3px;
                width: 120px;
                height: 36px;
                display: flex;
                align-items: center;
                justify-content: center;
                font-family: PingFangSC-Regular;
                font-size: 14px;
                color: #FFFFFF;
                &:hover{
                    cursor: pointer;
                }
            }
        }
    }
    .choice_job_btn_container{
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        .tuijian_title{
            padding-top: 50px;
            box-sizing: border-box;
            font-family: PingFangSC-Regular;
            font-size: 30px;
            color: #121C33;
            margin-bottom: 30px;
        }
        .job_type_item_container{
            width: 1200px;
            display: flex;
            justify-content: space-between;
            margin-bottom: 74px;
            .job_type_item{
                width: 380px;
                height: 130px;
                display: flex;
                align-items: center;
                padding-left: 40.1px;
                box-sizing: border-box;
                background-color: aquamarine;
                .left_img{
                    width: 99.8px;
                    height: 99.8px;
                    margin-right: 20.1px;
                }
            }
        }
    }
}
</style>
