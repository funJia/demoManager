<template>
    <!--地区联动-->
    <div class="regionalLinkage">
        <el-select v-model="pro" @change="pro_change()">
            <el-option v-for="v in pros" :label="v.name" :value="v.id"></el-option>
        </el-select>
        <el-select v-model="city" @change="city_change()">
            <el-option v-for="v in citys" :label="v.name" :value="v.id"></el-option>
        </el-select>
        <el-select v-model="county" ref="county" @change="county_change">
            <el-option v-for="v in countys" :label="v.name" :value="v.id"></el-option>
        </el-select>
    </div>
</template>
<script>
export default {
  props: ["provenId", "cityId", "areaId"],
  data() {
    return {
      all: [],
      pros: [],
      pro: "",
      citys: [],
      city: "",
      countys: [],
      county: ""
    };
  },
  computed: {
    appSett() {
      return this.$store.state.config.appSett;
    }
  },
  // created: function () {
  //     this.default();
  // },
  mounted() {
    // setTimeout(() => {
    //     this.default();
    // }, 1000);
  },
  methods: {
    default: function() {
      this.pros = this.next(1);
      //this.pros = this.next(19);//默认从湖南开始
      this.pro = 19; // this.pros[0]['id'];
      this.citys = this.next(this.pro);
      this.city = this.citys[0]["id"];
      this.countys = this.next(this.city);
      this.county = this.countys[0]["id"];
      this.$forceUpdate();
    },
    initComByPCA() {
      //根据省市区ID初始化组件
      if (!(this.provenId && this.cityId && this.areaId)) {
        this.default();
        return null;
      }
      let provenId = this.getId(this.provenId);
      let ctiyId = this.getChildId(this.cityId);
      let areaId = this.getId(this.areaId);

      this.pros = this.next(1);
      //this.pros = this.next(19);//默认从湖南开始
      this.pro = provenId; // this.pros[0]['id'];
      this.citys = this.next(this.pro);
      this.city = ctiyId;
      this.countys = this.next(this.city);
      this.county = areaId;
      this.$forceUpdate();
    },
    pro_change: function() {
      this.citys = this.next(this.pro);

      //判断当前选择的城市ID是否已存在
      let info = this.citys.filter(item => {
        if (item.id == this.city) {
          return true;
        } else {
          return false;
        }
      });

      if (!info || info.length == 0) {
        this.city = this.citys[0]["id"];
      }

      // this.countys = this.next(this.citys[0]['id']);
      // this.county = this.countys[0]['id'];
      this.$emit("proven", this.getRealId(this.pro), "");
    },
    city_change: function() {
      this.countys = this.next(this.city);
      //判断当前选择的区县ID是否已存在
      let info = this.countys.filter(item => {
        if (item.id == this.county) {
          return true;
        } else {
          return false;
        }
      });
      if (!info || info.length == 0) {
        this.county = this.countys[0]["id"];
      }
      this.$emit("city", this.getRealId(this.city), "");
      // this.$nextTick(() => {
      //     var label = this.$refs.county.selected.currentLabel;
      //     var value = this.$refs.county.selected.value;
      //     this.$emit('city', this.getRealId(value), label);
      // });
    },
    county_change: function() {
      this.$nextTick(() => {
        var label = this.$refs.county.selected.currentLabel;
        var value = this.$refs.county.selected.value;
        this.$emit("county", this.getRealId(value), label);
      });
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
          return this.all[i].realId;
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
