<template>
    <!--地区联动-->
    <div class="regionalLinkage">
        <el-select v-model="pro" @change="pro_change()">
            <el-option v-for="v in pros" :label="v.name" :value="v.id"></el-option>
        </el-select>
        <el-select v-model="city" @change="city_change()">
            <el-option v-for="v in citys" :label="v.name" :value="v.id"></el-option>
        </el-select>
    </div>
</template>
<script>
export default {
  props: ["provenId", "cityId"],
  data() {
    return {
      all: [],
      pros: [],
      pro: "",
      citys: [],
      city: ""
    };
  },
  computed: {
    appSett() {
      return this.$store.state.config.appSett;
    }
  },
  watch: {
    provenId(newVal) {
      let provenId = this.getId(newVal);
      this.pros = this.next(1);
      this.pro = provenId; // this.pros[0]['id'];
      this.citys = this.next(this.pro);
      // this.city = this.citys[0]['id'];
      let areaProvenInfo = this.getRealId(this.pro);
      this.$emit("proven", areaProvenInfo.realId, areaProvenInfo.name);
    },
    cityId(newVal) {
      let ctiyId = this.getChildId(newVal);
      this.city = ctiyId;
      let areaCityInfo = this.getRealId(this.city);
      this.$emit("city", areaCityInfo.realId, areaCityInfo.name);
    }
  },
  methods: {
    default: function() {
      this.pros = this.next(1);
      this.pro = this.pros[0]["id"];
      this.citys = this.next(this.pros[0]["id"]);
      this.city = this.citys[0]["id"];
      this.$forceUpdate();
    },
    initComByPCA() {
      //根据省市区ID初始化组件
      if (!(this.provenId && this.cityId)) {
        this.default();
        return null;
      }

      let ctiyId = this.getChildId(this.cityId);
      let provenId = this.getId(this.provenId);

      // let areaId = this.getId(this.areaId);

      this.pros = this.next(1);
      //this.pros = this.next(19);//默认从湖南开始
      this.pro = provenId; // this.pros[0]['id'];
      this.citys = this.next(this.pro);
      this.city = ctiyId;
      this.$forceUpdate();
    },
    pro_change: function() {
      this.citys = this.next(this.pro);
      this.city = this.citys[0]["id"];

      // this.countys = this.next(this.citys[0]['id']);
      // this.county = this.countys[0]['id'];
      let areaProvenInfo = this.getRealId(this.pro);
      this.$emit("proven", areaProvenInfo.realId, areaProvenInfo.name);
    },
    city_change: function() {
      // this.countys = this.next(this.city);
      // this.county = this.countys[0]['id'];
      let areaCityInfo = this.getRealId(this.city);

      this.$emit("city", areaCityInfo.realId, areaCityInfo.name);
    },
    next: function(id) {
      var re = [];
      for (var i in this.all) {
        if (this.all[i]["pid"] == id) {
          re.push(this.all[i]);
        }
      }
      return re;
    },
    getRealId(id) {
      //获取真实id
      for (var i = 0; i < this.all.length; i++) {
        if (id == this.all[i].id) {
          return this.all[i];
        }
      }
      return null;
    },
    getId(realId) {
      //获取唯一标识id
      for (var i = 0; i < this.all.length; i++) {
        if (realId == this.all[i].realId) {
          return this.all[i].id;
        }
      }
      return null;
    },
    getChildId(realId) {
      //获取城市唯一标识id
      for (var i = 0; i < this.all.length; i++) {
        if (realId == this.all[i].realId && this.all[i].pid != 1) {
          return this.all[i].id;
        }
      }
      return null;
    },
    getAreaInfo() {
      let that = this;
      $.getJSON(this.appSett['areaUrl'], res => {
        that.all = res;
        that.initComByPCA();
      });
    }
  },
  mounted() {
    this.getAreaInfo();
  }
};
</script>
