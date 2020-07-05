<template>
  <div>
    <h1>疫情信息管理平台</h1>
    <div class="box">
      <el-tag style="margin-left: 10px"
              v-for="item in items"
              :key="item.label"
              :type="item.type"
              effect="dark">
        {{ item.label }}
      </el-tag>

    </div>
    <hr style="width: 1000px ; border-bottom: 200px">
    <div >
      <el-form :inline="true"  class="demo-form-inline">
        <el-form-item label="国家名称查询">
          <el-input  placeholder="国家名称"></el-input>
        </el-form-item>
        <el-form-item label="确诊人数大于">
          <el-input  placeholder="确诊人数"></el-input>
        </el-form-item>
        <el-form-item label="治愈人数大于">
          <el-input  placeholder="治愈人数"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="handleAddRow()">增加</el-button>
        </el-form-item>
      </el-form>

    </div>
    <div class="box">
      <el-table
        ref="singleTable"
        :data="tableData.slice((currentPage-1)*pageSize,currentPage*pageSize)"
        highlight-current-row

        style="width: 100%"
        :header-cell-style="{

          'background-color':'#EEEE00',
          'color':'rgb(103,194,58)',
          'border-bottom':'3px rgb(103,194,58) solid'
        }"
      >
        <el-table-column
          type="index"
          width="50">
        </el-table-column>
        <el-table-column
          property="name"
          label="国家"
          width="120">
        </el-table-column>
        <el-table-column
          property="confirm"
          label="确诊人数"
          width="120">
        </el-table-column>
          <el-table-column
            property="confirmAdd"
            label="确诊人数新增"
            width="120">
        </el-table-column>
        <el-table-column
          property="dead"
          label="死亡人数"
          width="120">
        </el-table-column>
        <el-table-column
          property="deadCompare"
          label="死亡人数新增"
          width="120">
        </el-table-column>
        <el-table-column
          property="date"
          label="日期"
          width="120">
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)">删除</el-button>
          </template>
        </el-table-column>

      </el-table>
    </div>
    <div class="box">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total">
      </el-pagination>
    </div>
    <AddInfo :isAddInfo="isAddInfo" @addInfo="addInfo($event)"></AddInfo>
  </div>

</template>
<!--
confirm: 4899755
confirmAdd: 63749
dead: 320189
deadAdd: 4330
heal: 1876819
healAdd: 32397
lastUpdateTime: "2020-05-20 10:04:08"
nowConfirm: 2702747
nowConfirmAdd: 27030
-->
<script>
  import AddInfo from "./AddInfo.vue";
  export default {

    name: "Coronavirus",
    components: {AddInfo},
    data:function () {
      return {
        items: [
          { type: '', label: '全球累计确诊:' },
          { type: 'success', label: '全球累计确诊新增:' },
          { type: 'info', label: '全球累计死亡:' },
          { type: 'danger', label: '全球累计死亡新增:' },
          { type: 'warning', label: '全球累计治愈人数:' },
          { type: 'warning', label: '全球累计治愈新增:' }
        ],
        tableData: [],
        currentPage:1,
        pageSize:6,
        total:0,
        isAddInfo: {
          isShow:false
        }
      }
    },

    //生命周期函数
    created() {
      var that = this;
      //获取疫情数据
      this.$jsonp(" https://view.inews.qq.com/g2/getOnsInfo?name=disease_foreign").
      then(
        function (response) {

           var data = JSON.parse(response.data);
           console.log(data);
           //更新全球累计数据
          that.items[0].label += data.globalStatis.confirm;
          that.items[1].label += data.globalStatis.confirmAdd;
          that.items[2].label += data.globalStatis.dead;
          that.items[3].label += data.globalStatis.deadAdd;
          that.items[4].label += data.globalStatis.heal;
          that.items[5].label += data.globalStatis.healAdd;

          //拿到各个国家的数据
          that.tableData = data.foreignList;
          that.total = data.foreignList.length

        },
        function (err) {

        }
      )
    },
    methods: {
      handleEdit(index, row) {
        console.log(index, row);
      },
      handleDelete(index, row) {
        console.log(index, row);
        //取到当前页
        var currentPage = this.tableData.slice((this.currentPage-1)*this.pageSize,this.currentPage*this.pageSize);
        console.log(currentPage);
        this.tableData.splice(index,1);
        this.tableData.slice((this.currentPage-1)*this.pageSize,this.currentPage*this.pageSize);
        console.log(currentPage);
      },
      handleSizeChange(val) {
        this.pageSize = val;
      },
      handleCurrentChange(val) {
       this.currentPage=val;
      },
      handleAddRow(){
        this.isAddInfo.isShow = true;
      },
      addInfo(event){
        var month =  event.date.getMonth()+1;
        if (month<10){
          event.date = '0'+month+'.'+event.date.getDate();
        }else {
          event.date = month+'.'+event.date.getDate();
        }
        this.tableData.push(event);
        this.total = this.tableData.length
      }
    }

  }
</script>

<style scoped>
  h1{
    border-bottom: 2px solid #EEEE00;
    font-size: 25px;
    margin: 0 auto;
    padding: 5px;
    width: 1000px;
    text-align: center;
    color: #CD3278;

  }
  .box{
    width: 1000px;
    margin: 0 auto;
    margin-top: 20px;


  }



</style>
