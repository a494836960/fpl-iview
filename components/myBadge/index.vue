<template>
  <badge v-if="curData.style" :text="curData.name" :status="curData.style"></badge>
  <span v-else>{{curData.name}}</span>
</template>

<script>
  export default {
    props: ['list', 'target'],
    name: 'my-badge',
    data() {
      return {
        curData: {}
      }
    },

    methods: {
      getCurData(target) {
        this.list && this.list.map(item => {
          if (item.value == target) {
            this.curData = item;
          }
        })
      },
    },

    watch: {
      target: function (newValue) {
        this.getCurData(newValue);
      }
    },

    mounted() {
      this.getCurData(this.target);
    },

    created() {

    }
  }
</script>
