---
layout: post
title: Vue Image Update
date: 2020-01-07 23:29:44
tags:
- vue
- image upload
clearReading: true
thumbnailImage: 20200107.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Vue的手机拍照相册照片上传
一个小demo
支持手机相册和拍照上传
<!--more-->
```js
<template>
  <div>
    <input
      id="upload_file"
      type="file"
      style="display: none;"
      accept="image/*"
      name="file"
      @change="fileChange($event)"
    />
    <div class="upload_warp">
      <div class="upload_warp_img">
        <div class="upload_warp_img_div" v-for="(item, index) in imgList">
          <div class="upload_warp_img_div_top">
            <img
              src="data:image/png;base64,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"
              class="upload_warp_img_div_del"
              @click="fileDel(index)"
            />
          </div>
          <img :src="item.file.src" style="display: inline-block;" />
        </div>
        <div
          class="upload_warp_left"
          id="upload_warp_left"
          @click="fileClick()"
          v-if="this.imgList.length < 6"
        >
          <!--<img src="../assets/upload.png">-->
          <div>相册上传</div>
        </div>
      </div>
    </div>
    <div class="upload_warp_text">
      <span
        >选中{{ imgList.length }}张文件，共{{ bytesToSize(this.size) }}</span
      >
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    name: "cameras-and-albums",
   data(){
    return{
     imgList: [],
     datas: new FormData(),
     files:0,
     size:0
    }
   },
   methods:{
    //调用相册&相机
    fileClick() {
     $('#upload_file').click();
    },
    //调用手机摄像头并拍照
    getImage() {
     let cmr = plus.camera.getCamera();
     cmr.captureImage(function(p) {
      plus.io.resolveLocalFileSystemURL(p, function(entry) {
       compressImage(entry.toLocalURL(),entry.name);
      }, function(e) {
       plus.nativeUI.toast("读取拍照文件错误：" + e.message);
      });
     }, function(e) {
     }, {
      filter: 'image'
     });
    },
    //从相册选择照片
    galleryImgs() {
     plus.gallery.pick(function(e) {
      let name = e.substr(e.lastIndexOf('/') + 1);
      compressImage(e,name);
     }, function(e) {
     }, {
      filter: "image"
     });
    },
    //点击事件，弹出选择摄像头和相册的选项
    showActionSheet() {
     let bts = [{
      title: "拍照"
     }, {
      title: "从相册选择"
     }];
     plus.nativeUI.actionSheet({
       cancel: "取消",
       buttons: bts
      },
      function(e) {
       if (e.index == 1) {
        this.getImage();
       } else if (e.index == 2) {
        this.galleryImgs();
       }
      }
     );
    },
    fileChange(el) {
     this.files=$("#upload_file").get(0).files;
     console.log(this.files.length);
     for(let i=0;i<this.files.length;i++){
      this.datas.append("file",this.files[i]);
     }
     this.show1=false;
     console.log(typeof this.files);
     console.log(this.files);
     if (!el.target.files[0].size) return;
     this.fileList(el.target);
     el.target.value = ''
    },
    fileList(fileList) {
     let files = fileList.files;
     for (let i = 0; i < files.length; i++) {
      //判断是否为文件夹
      if (files[i].type != '') {
       this.fileAdd(files[i]);
      } else {
       //文件夹处理
       this.folders(fileList.items[i]);
      }
     }
    },
    //文件夹处理
    folders(files) {
     let _this = this;
     //判断是否为原生file
     if (files.kind) {
      files = files.webkitGetAsEntry();
     }
     files.createReader().readEntries(function (file) {
      for (let i = 0; i < file.length; i++) {
       if (file[i].isFile) {
        _this.foldersAdd(file[i]);
       } else {
        _this.folders(file[i]);
       }
      }
     })
    },
    fileAdd(file) {
     //总大小
     this.size = this.size + file.size;
     //判断是否为图片文件
     if (file.type.indexOf('image') == -1) {
      file.src = 'wenjian.png';
      this.imgList.push({
       file
      });
     } else {
      let reader = new FileReader();
      reader.vue = this;
      reader.readAsDataURL(file);
      reader.onload = function () {
       file.src = this.result;
       this.vue.imgList.push({
        file
       });
      }
     }
    },
    fileDel(index) {
     this.size = this.size - this.imgList[index].file.size;//总大小
     this.imgList.splice(index, 1);
    },
    bytesToSize(bytes) {
     if (bytes === 0){
      return '0 B';
     }
     let k = 1000, // or 1024
      sizes = ['B', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'],
      i = Math.floor(Math.log(bytes) / Math.log(k));
     return (bytes / Math.pow(k, i)).toPrecision(3) + ' ' + sizes[i];
    },
    dragenter(el) {
     el.stopPropagation();
     el.preventDefault();
    },
    dragover(el) {
     el.stopPropagation();
     el.preventDefault();
    },
    drop(el) {
     el.stopPropagation();
     el.preventDefault();
     this.fileList(el.dataTransfer);
    },
    shows(et,tx){
     this.strut=et;
     this.txt=tx;
    },
    handleClick(){
     this.$store.commit('add')
    },
   },
  }
</script>
<style lang="">
    .upload_warp_img_div_del {
    position: absolute;
    top: 6px;
    width: 16px;
    right: 4px;
}
.upload_warp_img_div_top {
    position: absolute;
    top: 0;
    width: 100%;
    height: 30px;
    background-color: rgba(0, 0, 0, 0.4);
    line-height: 30px;
    text-align: left;
    color: #fff;
    font-size: 12px;
    text-indent: 4px;
}
.upload_warp_img_div_text {
    white-space: nowrap;
    width: 80%;
    overflow: hidden;
    text-overflow: ellipsis;
}
.upload_warp_img_div img {
    max-width: 100%;
    max-height: 100%;
    vertical-align: middle;
}
.upload_warp_img_div {
    position: relative;
    height: 100px;
    width: 120px;
    border: 1px solid #ccc;
    margin: 0px 5px 5px 0px;
    float: left;
    line-height: 100px;
    display: table-cell;
    text-align: center;
    background-color: #eee;
    cursor: pointer;
}
.upload_warp_img {
    border-top: 1px solid #D2D2D2;
    padding: 5px 0 0 5px;
    overflow: hidden
}
.upload_warp_text {
    text-align: left;
    margin-bottom: 5px;
    padding-top: 5px;
    text-indent: 14px;
    border-top: 1px solid #ccc;
    font-size: 14px;
}
.upload_warp_right {
    float: left;
    width: 57%;
    margin-left: 2%;
    height: 100%;
    border: 1px dashed #999;
    border-radius: 4px;
    line-height: 130px;
    color: #999;
}
.upload_warp_left button {
    margin: 8px 5px 0px 5px;
    cursor:pointer;
}
.upload_warp_left {
    float: left;
}
.upload_warp {
    margin: 5px;
}
</style>
```
