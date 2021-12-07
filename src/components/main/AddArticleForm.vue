<!--
 * @Author: xiaozhangtx
 * @Date: 2021-11-18 18:54:03
 * @LastEditTime: 2021-11-19 08:21:24
 * @LastEditors: Please set LastEditors
 * @Description: 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
 * @FilePath: \webport\src\components\main\AddArticleForm.vue
-->
<template>
  <div>
    <!-- 添加文章对话框 -->
    <el-dialog title="发表文章" :visible.sync="addDialogVisible" width="50vh" @colse="addDialogClosed">
      <el-form :model="articleForm" :rules="articleFormRules" ref="articleFormRef" label-width="30%"
        width="90%">
        <!-- 文章名称 -->
        <el-form-item label="文章标题" prop="name">
          <el-input v-model="articleForm.name" placeholder="请输入文章标题"></el-input>
        </el-form-item>
        <!-- 文章内容 -->
        <el-form-item label="文章内容" prop="content">
          <el-input type="textarea" :autosize="{ minRows: 2, maxRows: 4}" placeholder="请输入文章内容"
            v-model="articleForm.content">
          </el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <a-button @click="addDialogClosed">取 消</a-button>
        <a-button type="primary" style="margin-left: 20px" @click="addArticle">确 定</a-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      // 控制添加文章对话框显示/隐藏
      addDialogVisible: false,
      // 添加文章信息
      articleForm: {
        name: '',
        content: '',
        uploaded: new Date(),
        username: '',
      },
      // 添加文章表单验证规则
      articleFormRules: {
        name: [{ required: true, message: '请输入文章标题', trigger: 'blur' }],
        content: [{ required: true, message: '请输入文章内容', trigger: 'blur' }],
      },
    }
  },
  methods: {
    // 打开对话框
    showAddArticleDialog(username) {
      this.addDialogVisible = true
      this.articleForm.username = username
    },
    // 关闭窗口
    addDialogClosed() {
      this.$refs.articleFormRef.resetFields()
      this.addDialogVisible = false
    },
    // 确认添加
    addArticle() {
      this.$refs.articleFormRef.validate(async (valid) => {
        console.log(valid)
        if (!valid) return
        console.log(this.articleForm)
        // 发起请求
        const { data: res } = await this.$http.put('addarticle', this.articleForm)
        if (res.code == 200) {
          //隐藏
          this.addDialogVisible = false
          this.$refs.articleFormRef.resetFields()
          this.$parent.getArticleList()
          return this.$message.success(res.message)
        } else {
          return this.$message.error(res.message)
        }
      })
    },
  },
}
</script>
