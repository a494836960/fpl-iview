<script>
  export default {
    data() {
      return {
        isShow: false,
        modal_loading: false,
      }
    },
    methods: {
      showModal() {
        this.isShow = true
      },
      closeModal() {
        this.isShow = false
        this.$refs[this.formName].resetFields();
      },
      validateForm() {
        return new Promise((resolve) => {
          this.$refs[this.formName].validate((valid) => {
            if (valid) {
              resolve(true);
            }
          })
        })
      }
    }
  }
</script>
