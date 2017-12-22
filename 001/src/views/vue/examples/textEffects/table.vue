<template>
  <div class="table-demo">
    <h3>表格中文字效果</h3>
    <h5>自定义效果和官方提供的效果</h5>
      <el-row>
          <el-col :span="4">
                    <el-table :data="tableList">
        <el-table-column label="介绍1">
            <template slot-scope="scope">
        <table-cell-ellipsis :text="scope.row.intro1"></table-cell-ellipsis>
            </template>
          </el-table-column>          
          <el-table-column label="介绍2" prop="intro2" show-overflow-tooltip="true">
          </el-table-column>
      </el-table>
          </el-col>
      </el-row>
  </div>
</template>
<script>
import TableCellEllipsis from "@/views/vue/components/textEffects/table-cell-ellipsis.vue";
export default {
  data() {
    return {
      tableList: [
        {
          intro1: "4444554444555533344445555333444455553334444555533355333",
          intro2: "44445554444555533344445555333444455553335333"
        }
      ]
    };
  },
  components: {
    TableCellEllipsis
  }
};
</script>
<style lang="less">
.table-demo{
  margin-top:20px;
  margin-left:20px;
}
</style>

