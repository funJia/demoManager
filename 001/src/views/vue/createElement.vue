<script>
//文字效果在表格中的效果
import TETable from "./examples/textEffects/table.vue";
//地区选择在表单中的效果
import AreaForm from "./examples/areaEffects/form.vue";

export default {
  render: function(createElement) {
    var that = this;
    switch (that.name) {
      case "TETable":
        return createElement(TETable, {});
        break;
      case "AreaForm":
        return createElement(AreaForm, {});
        break;        
      default:
        console.log("未找到组件");
        break;
    }
  },
  props: {
    "name": {
      type: Number,
      default: 0
    }
  }
};
</script>