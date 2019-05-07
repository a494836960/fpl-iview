<template>
  <div :class="this.cls" class="box">
    <h6 class="title">
      <Divider orientation="left">{{title}}</Divider>
    </h6>
    <div class="box-content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['title', 'cls'],
    name: 'fpl-box',
    data() {
      return {};
    }
  }

</script>

<style lang="less">
  .box {
    position: relative;
    padding: 10px;
    background: #fff;
    border-radius: 5px;

    .box-content {
      padding: 0 20px;
    }

    .title {
      .ivu-divider-inner-text {
        display: inline-block;
        background-color: rgba(42, 130, 228, 1);
        padding: 5px 14px;
        vertical-align: middle;
        font-size: 14px;
        color: #fff;
        margin-left: -20px;
      }

      .ivu-divider-horizontal.ivu-divider-with-text-left {
        &:before {
          display: none;
        }

        &:after {
          color: rgba(80, 80, 80, 1);
          border-top: 3px solid rgba(214, 225, 236, 1);
          border-radius: 3px;
          height: 5px;
          left: 5px;
        }
      }
    }
  }

  .box-login-line { //登录框
    .box-content {
      padding: 0;
    }

    .line {
      position: absolute;
      right: 20px;
      left: 60px;
      top: 30px;
    }
  }
</style>
