<!-- 
    图片压缩上传小列子 
-->
<template>
    <div class="wrap">
        <div class="addContent" v-for="(item, index) in objImgPath" :key='index'>
            <div class="wrap_img"  >
                <img :src="getObjectURL(item)">
            </div>
            <div class="cols" @click="getClos(index)">X</div>
        </div>
        <div class="addContent" v-show="Object.keys(objImgPath).length < 3">
            <input type="file"  accept="image/*" @change="compressImg" capture="camera" />
            <div class="add">+</div>
        </div>
        <input class="but" type="button" value="提交" @click="addCommit">
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data(){
        return {
            formData: new FormData(),
            objImgPath: {},
            objKey: '',
        }
    },
    methods:{
        compressImg(e){
            if (e.target.files){
            lrz( e.target.files[0])  
                    .then((rst) => {
                        let newFile = new File([rst.file],this.uuid()+'.'+rst.origin.name.split('.')[1],{ type: "multipart/form-data" })
                        console.log(rst)
                        this.formData.append('file', newFile)
                        this.formData.set('path', 'logo')
                        this.$set(
                            this.objImgPath, 
                            this.key = rst.origin.name,
                            rst.file
                        )
                        //成功时执行
                    }).catch((error) => {
                        console.log(error)
                        //失败时执行
                    }).always(() =>  {
                        //不管成功或失败，都会执行
                    })
            };
        },
        uuid() {
            var s = [];
            var hexDigits = "0123456789abcdef";
            for (var i = 0; i < 36; i++) {
                s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
            }
            s[14] = "4"; 
            s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1);
            s[8] = s[13] = s[18] = s[23] = "-";
            var uuid = s.join("");
            return uuid.replace(/-/g,"")
        },
        getClos(index){
            delete this.objImgPath[index]
            this.$forceUpdate()
        },
        // 接收图片源信息， 转成路径
        getObjectURL(file) {
            var url = null;
            if (window.createObjectURL != undefined) {
                // basic
                url = window.createObjectURL(file);
            } else if (window.URL != undefined) {
                // mozilla(firefox)
                url = window.URL.createObjectURL(file);
            } else if (window.webkitURL != undefined) {
                // webkit or chrome
                url = window.webkitURL.createObjectURL(file);
            }
            return url;
        },
        addCommit(){
            let url = ''
            axios.post(url, this.formData).then( (res) => {
                console.log(res)
            })
        }
    }
}
</script>

<style scoped>
.wrap{
    display: flex;
    padding-left: 30px;
    flex-wrap: wrap;
    position: relative;
}
.addContent{
    width:87px;
    height:104px;
    border-radius:4px;
    border:1px solid rgba(219,219,219,1);
    position: relative;
    text-align: center;
    margin:5px;
}
.addContent .add{
    font-size: 50px;
}
.addContent .cols{
    width:20px;
    height:20px;
    font-size: 16px;
    position: absolute;
    top:-10px;
    right:-5px;
    border:1px solid #ccc;
    border-radius: 50%;
}
.addContent input{
    width:100%;
    height:100%;
    opacity: 0;
    position: absolute;
    top:0;
    left:0;
}
.wrap_img{
    overflow: hidden;
    width:100%;
    height:100%;
    position: absolute;
    top:50%;
    left:0;
    transform: translateY(-50%);
}
.wrap_img img{
    width:100%;
    height:auto;
}
.but{
    width:100px;
    height:45px;
    display: block;
    position: fixed;
    bottom: 40px;
    left:50%;
    transform: translateX(-50%);
    border:0;
    background: #5c8ce7;
    font-size: 14px;
    color:#fff;
}
</style>
