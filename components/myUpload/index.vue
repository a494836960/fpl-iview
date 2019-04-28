<!-- 单文件上传 -->
<template>
  <div>
    <div class="demo-upload-list" v-for="(item, index) in uploadList">
      <template v-if="item.status === 'finished'">
        <img :src="item.url">
        <div class="demo-upload-list-cover">
          <Icon type="ios-eye-outline" @click.native="handleView(item.url)"></Icon>
          <Icon type="ios-trash-outline" @click.native="handleRemove(index)"></Icon>
        </div>
      </template>
      <template v-else>
        <Progress v-if="item.showProgress" :percent="item.percentage" hide-info></Progress>
      </template>
    </div>
    <Upload
      ref="upload"
      :show-upload-list="false"
      :default-file-list="defaultImgs"
      :on-success="handleSuccess"
      :format="['jpg','jpeg','png']"
      :max-size="2048"
      :on-format-error="handleFormatError"
      :on-exceeded-size="handleMaxSize"
      type="drag"
      :data="data"
      :headers="{'Authorization':'Bearer '+ token}"
      :action="baseUrl+action"
      :style="`display: ${uploadList.length == 0?'inline-block': 'none'};width:58px;`">
      <div style="width: 58px;height:58px;line-height: 58px;">
        <Icon type="ios-camera" size="20"></Icon>
      </div>
    </Upload>
    <Modal title="View Image" v-model="visible">
      <img :src="imgUrl" v-if="visible" style="width: 100%">
    </Modal>
  </div>
</template>
<script>
  import {baseUrl} from '@/config/env'

  export default {
    props: ['action', 'data', 'defaultList'],
    data() {
      let token = this.$store.getters.access_token
      return {
        defaultImgs:[],
        imgUrl: '',
        token: token,
        visible: false,
        uploadList: []
      }
    },
    watch: {
      defaultList: function (newValue, oladValue) {
        let list = [];
        newValue.map(item => {
          if (item.url) {
            list.push(item);
          }
        });
        this.defaultImgs = list;
        this.$nextTick(() => {
          this.uploadList = this.$refs.upload.fileList;
        });
      }
    },
    methods: {
      handleView(imgUrl) {
        this.imgUrl = imgUrl;
        this.visible = true;
      },
      handleRemove(index) {
        this.$refs.upload.fileList.splice(index, 1);
        this.$emit('success', {});
      },
      handleSuccess(res, file) {
        console.log(res);
        file.url = res.data;
        this.$emit('success', file);
      },
      handleFormatError(file) {
        this.$Notice.warning({
          title: '文件格式不正确',
        });
      },
      handleMaxSize(file) {
        this.$Notice.warning({
          title: '文件太大，最大2M',
        });
      },
    },
    mounted() {
      this.uploadList = this.$refs.upload.fileList;
    }
  }
</script>
<style>
  .demo-upload-list {
    display: inline-block;
    width: 60px;
    height: 60px;
    text-align: center;
    line-height: 60px;
    border: 1px solid transparent;
    border-radius: 4px;
    overflow: hidden;
    background: #fff;
    position: relative;
    box-shadow: 0 1px 1px rgba(0, 0, 0, .2);
    margin-right: 4px;
  }

  .demo-upload-list img {
    width: 100%;
    height: 100%;
  }

  .demo-upload-list-cover {
    display: none;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, .6);
  }

  .demo-upload-list:hover .demo-upload-list-cover {
    display: block;
  }

  .demo-upload-list-cover i {
    color: #fff;
    font-size: 20px;
    cursor: pointer;
    margin: 0 2px;
  }
</style>
