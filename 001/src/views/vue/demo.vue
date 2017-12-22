<template>
  <div>
      <ul>
          <li>
              <el-button type="text" @click="show=true;name='TETable'">文字省略效果</el-button>
          </li>
          <li>
              <el-button type="text" @click="show=true;name='AreaForm'">地区选择</el-button>
          </li>          
      </ul>
        <CreateElement :name="name"></CreateElement>
  </div>
</template>
<script>
import CreateElement from "./createElement.vue";
export default {
  data() {
    return {
      show: false,
      name: ""
    };
  },
  components: {
    CreateElement
  }
};
</script>

