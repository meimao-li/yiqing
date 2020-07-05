<template>
    <div>
      <el-dialog title="添加国家疫情信息" :visible.sync="isAddInfo.isShow">
        <el-form :model="form" label-width="100px" :rules="rules" ref="ruleForm">
          <el-form-item label="国家名称" prop="name">
            <el-input v-model="form.name" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="确诊人数" >
            <el-input v-model="form.confirm" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="确诊人数新增" >
            <el-input v-model="form.confirmAdd" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="死亡人数" >
            <el-input v-model="form.dead" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="死亡人数新增" >
            <el-input v-model="form.deadCompare" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="日期" >
            <el-date-picker
              v-model="form.date"
              type="date"
              placeholder="选择日期">
            </el-date-picker>
          </el-form-item>

        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="isAddInfo.isShow = false">取 消</el-button>
          <el-button type="primary" @click="addInfo('ruleForm')">确 定</el-button>
        </div>
      </el-dialog>
    </div>
</template>

<script>
    export default {
        name: "AddInfo",
        props:{
          isAddInfo:Object
        },
      data : function () {
        return{
          form: {
            name: '',
            confirm: '',
            confirmAdd:'',
            dead:'',
            deadCompare:'',
            date:''

          },
          rules: {
            name: [
              { required: true, message: '请输入国家名称', trigger: 'blur' },
              { min: 3, max: 5, message: '请输入国家名称，不能为空', trigger: 'blur' }
            ]
          }
        }
      },
      methods:{
        addInfo(formName){


          this.$refs[formName].validate((valid) => {
            if (valid) {
              alert('submit!');
              //子组件向父组件传值,自定义事件
              this.$emit("addInfo",this.form);
              this.isAddInfo.isShow=false; //隐藏添加对话框
              this.form = '';
            } else {
              console.log('error submit!!');
              return false;
            }
          });
        }
      }


    }
</script>

<style scoped>

</style>
