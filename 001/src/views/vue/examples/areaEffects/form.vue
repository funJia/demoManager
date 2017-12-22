<template>
  <div>
<el-form ref="form" label-width="150px">
  <el-form-item label="地区二级联动">      
    <regional-linkage2 ref="rl2" @proven="changeProven" @city="changeCity" :provenId="proven" :cityId="city"></regional-linkage2>
  </el-form-item>
  <el-form-item label="地区三级联动">
    <regional-linkage3 ref="rl3" @proven="changeProven" @city="changeCity" @county="changeArea" :provenId="proven" :cityId="city" :areaId="areaId"></regional-linkage3>    
  </el-form-item>
</el-form>
  </div>
</template>
<script>
import RegionalLinkage2 from "@/views/vue/components/areaEffects/regionalLinkage2.vue";
import RegionalLinkage3 from "@/views/vue/components/areaEffects/regionalLinkage3.vue";
export default {
  data() {
    return {
      proven: 18, //开户所在省份
      provenName: "",
      city: 187, //开户城市
      cityName: "",
      areaId: ""
    };
  },
  components: {
    RegionalLinkage2,
    RegionalLinkage3
  }
};
</script>

