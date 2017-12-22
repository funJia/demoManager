<template>
<!-- 表格列中文字省略效果 -->
  <div class="table-cell-ellipsis" :title="text">
    {{text||'--'}}
  </div>
</template>
<script>
export default {
  props: ["text"]
};
</script>
<style lang="less">
.table-cell-ellipsis {
  cursor: pointer;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>


