<template>
<div>
    <div class="background">
        <!-- 输出的gif放在前景后面，背景前面 -->
        <img
            id="gif"
            style="visibility:hidden;"
        />
    </div>
    <div class="frontground">
        <!-- 1.上传mp4文件 -->
        <div class="uploadMP4">
            <form class="form" id="uploadForm" method="post" action="" enctype="multipart/form-data">
                <input type="file" id="id_file" name="file" mutiple='' accept=".mp4"/>
                <input type="button" value="上传mp4" id="submit" @click="check()">
            </form>
        </div>

        <!-- 2.后端运行 -->
        <div class="run">
            <button id="run" @click="rundance()">开始运行</button>
        </div>

        <!-- 3.下载gif文件 -->
        <div class="download">
            <button id="download" @click="download()">下载文件</button>
        </div>

        <!-- 4.展示效果 -->
        <div class="openTV">
            <button class="primary-btn" @click="myshow()">播  放</button>
        </div>

        <!-- 一些静态图片放在前景前面 -->
        <img
            id="loading"
            src="@/assets/load.png"
            style="visibility:hidden;"
        />
        <img
            id="mote"
            src="@/assets/mote.png"
        />
        <button @click="test()">test</button>

    </div>


</div>
</template>
<script>
// @ is an alias to /src
// 也可以直接用'../api/api'
import {runDance,downloadFile} from '@/api/api'

export default {
    name:'Front',
    data(){
        return{
            download_ok:0,
        }
    },
    methods:{
        test(){
            console.log('test')
            this.$axios.get('/requesttest/t1/').then(
                (response) => {
                    console.log("axios.get('/requesttest/t1/').then response.data:",response.data);
                    return response
                }
            )    
            $.ajax({
                type:'get',
                url:'api/requesttest/t1/'
            }).then((data)=>{
                console.log('ajax ok')
            })
        },
        // 0.显示/停止显示加载中.png
        show_load(){
            var loading = document.getElementById('loading');
            loading.style.visibility = 'visible'
        },
        stop_load(){
            var loading = document.getElementById('loading');
            loading.style.visibility = 'hidden'
        },
        // 1.上传文件
        check(){
            this.show_load()
            var doc = document.getElementById('id_file')
            for(var i=0; i<doc.files[i].length; i++){
                var name = doc.files[i].name
                var hz = name.substring(name.lastIndexOf(".")+1)
                if(hz!="mp4"){
                    alert("错误，请上传mp4")
                    return false
                }
            }
            this.upload()
        },
        upload(){
            var formData = new FormData($('#uploadForm')[0])
            console.log('$(\'#uploadForm\')',$('#uploadForm')) // 返回一个jquery元素 jQuery.fn.init [form#uploadForm.form]
            console.log('$(\'#uploadForm\')[0]',$('#uploadForm')[0]) // 获得了整个form组件（输出的是form的html但是没有其中的两个input内容）
            console.log('formData',formData) // FormData {}
            $.ajax({
                type:'post',
                url:'api/api/fileUpload/upload/', // 因为没使用axios所以要加两个api/才能进代理服务器
                data: formData,
                cache: false, //是否在缓存中读取数据的读取
                processData: false, //用于对data参数进行序列化处理
                conteneType: false, //发送信息至服务器时内容编码类型,上传文件时设置为false
            }).then((data)=>{
                console.log("上传文件返回信息data:",data)
                console.log("data.msg",data.msg)
                if(data.msg=='Success'){
                    alert("上传成功")
                }else if(data==1){
                    alert("错误,上传失败！",'error',function(){});
                }else{ 		
                    alert("错误,请选择需要上传的文件！",'error',function(){});   		
                }
                this.stop_load()
            })
        },
        // 2.运行
        rundance(){
            this.show_load()
            runDance().then(()=>{
                this.stop_load()
            })
        },
        // 3.下载
        download(){
            this.show_load()
            downloadFile().then(()=>{
                this.stop_load()
                this.download_ok = 1
            })
        },
        // 4.播放
        myshow(){
            if(this.download_ok == 1){
                var gif = document.getElementById('gif');
                gif.src = require('../assets/dance.gif'.replace('on','off'))
                gif.style.visibility = 'visible' // 另一个是hidden
            }else{
                alert('请先下载gif到:\'./src/assets/dance.gif\'')
            }
        },
    }
}
</script>
<style>
.background{
    position: absolute;
    width: 210vh;
    height: 100vh;
    background: url("~@/assets/my_bg.png") center center no-repeat;
    background-size: 100% 100%;
}
#gif{
    position: absolute;
    top: 48%;
    left: 53%;
    width:47.5%;
    height:49%;
    opacity: 1;
    visibility: hidden;
}
.frontground{
    position: absolute;
    width: 210vh;
    height: 100vh;
    background: url("~@/assets/my_fg.png") center center no-repeat;
    background-size: 100% 100%;
}
#id_file{
    position: absolute;
    top: 37%;;
    left: 15%;
}
#submit{
    position: absolute;
    top: 37%;;
    left: 40%;
}
#run{
    position: absolute;
    top: 50%;
    left: 15%;
}
#download{
    position: absolute;
    top: 63%;
    left: 15%;
}
.primary-btn {
    /* 字体 */
    font-size: 19px;
    color: #4cd2ff; 
    line-height: 1.2;
    text-align: center;
    /* 边框 */
    border: 5px solid #4cd2ff;
    border-radius: 2px;
    cursor: pointer;
    transition: opacity 0.2s;
    outline: none;
    /* 位置 */
    position: absolute;
    top: 21.3%;
    left: 70.3%;
    width: 13.4%;
    height: 12.3%;
/* 好看的装饰，在字上面加两个蓝色爱心 */
  /* text-emphasis-style: '❤';
  text-emphasis-color: #4cd2ff; */
}
.primary-btn::before {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 100%;
    height: 100%;
    background-color: #000;
    border: inherit;
    border-radius: inherit;
    transform: translate(-50%, -50%);
    opacity: 0;
    content: ' ';
}
.primary-btn:active::before {
    opacity: 0.1;   
}
#loading{
  position: absolute;
  top: 70%;
  left: 75%;
  opacity: 1;
  width:5%;
}
#mote{
  position:absolute;
  top: 23.15%;
  left: 89.58%;
  opacity: 1;
  width:7.92%;
}
</style>