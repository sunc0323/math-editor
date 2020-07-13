<template>
   <div class="editor-dialog">
       <!-- 弹框 -->
       <el-dialog
            title="编辑器"
            :closeOnClickModal="false"
            :visible.sync="editorVisiable"
            :before-close="closeDialog"
            append-to-body
            width="800px">
            <div id="mathEditor"></div>
            <span slot="footer" class="dialog-footer">
              <el-button @click="closeDialog()">取消</el-button>
              <el-button type="primary" @click="getFormulaContent">确定</el-button>
            </span>
        </el-dialog>
   </div>
</template>
<script>
    export default {
        data () {
            return {
                mathField:null,
                kfeditor:null,
            }
        },
        props: {
            editorVisiable: {
                type: Boolean,
                required: true,
                default: false
            },
            content: { // 编辑器的内容
                type: String,
                default: '',
                required: true
            }
        },
        watch: {
            editorVisiable: {
                handler(val) {
                   if(val){
                    this.$nextTick(() => {
                      let _self = this
                      jQuery( ($)=> {
                        if(_self.kfeditor){
                          _self.initMathContent();
                        }else{
                          var factory = kf.EditorFactory.create( $( `#mathEditor` )[ 0 ], {
                              render: {
                                  fontsize: 40
                              },
                              resource: {
                                  path: "./resource/"
                              }
                          } );
                          factory.ready( function ( KFEditor ) {
                              _self.kfeditor = this;
                              _self.initMathContent();
                              _self.bindInsertMath();
                          });
                        }
                      });
                    })
                  }
                },
                immediate: false
            }
        },
        mounted() {
          if(this.kfeditor){
            this.bindInsertMath();
          }
        },
        methods: {
            initMathContent(){
              if(!this.mathField){
                let config={
                }
                var MQ = MathQuill.getInterface(2);
                var htmlElement = $('.kf-editor-canvas-container')[0];
                $('.kf-editor-canvas-container').html("");
                this.mathField = MQ.MathField(htmlElement, config);
              }
              if(this.content){
                this.mathField.latex(this.content);
              }else{
                this.mathField.latex('');
              }
              setTimeout(()=>{
                this.mathField.focus();
              },100);
            },
            bindInsertMath(){
              let _self=this;
              this.kfeditor.registerService( "control.insert.string", null, {
                insertStr: (str)=>{
                  _self.mathField.write(str);
                }
              });
            },
            closeDialog(){
              this.$emit('update:visible', false);
            },
            // 获取编辑器内容
            getFormulaContent() {
                if (this.kfeditor) {
                  if(this.mathField.latex()){
                    this.$emit('getContentFromEditor', this.mathField.latex());
                  }else{
                    this.$emit('getContentFromEditor','');
                  }
                }
                else {
                    this.$message.warning('请刷新页面');
                }
            }
        },
       
    }
</script>
<style lang="scss">

</style>