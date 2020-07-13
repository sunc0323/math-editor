<template>
    <span class="mathquill-latex" ref="mathquill">{{formula}}</span>
</template>
<script>
    export default {
        data () {
            return {
              staticMath:null
            }
        },
        props: {
          formula: {
            type: String
          }
        },
        watch: {
          formula:{
            handler(val) {
              console.log(val)
              this.staticMath&&this.staticMath.latex(val)
            }
          }
        },
        mounted() {
          var MQ = MathQuill.getInterface(2);
          this.staticMath=MQ.StaticMath(this.$refs.mathquill);
        },
        methods: {
            
        }
    }
</script>
<style lang="scss" scope>
.mathquill-latex{
  .mq-empty{
    display: none !important;
  }
}
</style>