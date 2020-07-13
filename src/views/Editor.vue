<template>
  <div class="editor-container">
    <el-row>
      <el-col :span="12">
        <el-button type="primary" plain size="small" @click="showDialog('first')">打开公式编辑器</el-button>
        <div>
          预览:<vue-mathquill :formula="form.first"></vue-mathquill>
        </div>
        <div>
          LaTeX:{{form.first}}
        </div>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="12">
        <el-button type="primary" plain size="small" @click="showDialog('second')">打开公式编辑器</el-button>
        <div>
          预览:<vue-mathquill :formula="form.second"></vue-mathquill>
        </div>
        <div>
          LaTeX:{{form.second}}
        </div>
      </el-col>
    </el-row>
    <editorDialog
      :editorVisiable="editorVisiable"
      :visible.sync="editorVisiable"
      :content="currentContent"
      @getContentFromEditor="getContentFromEditor"
    ></editorDialog>
  </div>
</template>
<script>
import editorDialog from "./mathEditor/editorDialog";
import vueMathquill from './mathEditor/vueMathquill'

export default {
  components: {
    editorDialog,
    vueMathquill
  },
  data() {
    return {
      editorVisiable: false, // 是否显示编辑弹窗
      dataKey: "", // 数据的关键字
      form: {
        first: "",
        second:""
      }
    };
  },
  computed: {
     currentContent(){
      return this.form[this.dataKey]
    }
  },
  methods: {
    showDialog(key){
      this.dataKey=key;
      this.editorVisiable = true;
    },
    // 从子编辑器获取内容
    getContentFromEditor(content) {
      if(content){
        this.form[this.dataKey] = content;
      }else{
        this.form[this.dataKey] ="";
      }
      this.editorVisiable = false;
    }
  }
};
</script>
 
<style lang="scss" >
.MathJax{
  outline: none !important;
}
.el-row {
    margin: 50px 0;
    .ql-container.ql-snow {
        min-height: 200px;
    }
}

</style>
 