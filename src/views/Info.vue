<!--  -->
<template>
  <div class="info">
    <van-nav-bar title="居民登记" fixed left-arrow />

    <div class="card">
      <van-cell title="所属小区" class="bt" :value="info.villageName">
      </van-cell>

      <van-cell :value="info.buildNum" class="bt" title="楼栋号" />

      <van-cell :value="info.numberPlate" class="bt" title="门牌号" />

      <van-cell :value="info.housesType" class="bt" title="房屋类型" />
    </div>
    <div class="card">
      <van-cell :value="info.residentType" class="bt" title="居民类型" />

      <van-cell :value="info.title" class="bt" title="姓名" />

      <van-cell :value="info.sexType" class="bt" title="性别" />

      <van-cell
        title="出生日期"
        class="bt"
        :value="info.birthdayFilter"
      ></van-cell>

      <van-cell :value="info.identityCard" class="bt" title="身份证" />

      <van-cell :value="info.tel" class="bt" title="手机号" />

      <van-cell title="文化程度" class="bt" :value="info.levelOfEducation"></van-cell>

      <van-cell title="党员是否" class="bt" :value="info.isDy"></van-cell>
    </div>

    <div class="card">
      <van-cell title="与户主关系" class="bt" :value="info.relation"></van-cell>

      <van-cell :value="info.address" class="bt" title="户口所在地" />

      <van-cell title="户口变动日期" :value="info.updateTimeFilter"></van-cell>
    </div>

    <div class="card">
      <van-cell :value="info.tag" title="特殊标签" />
    </div>

    <div class="card">
      <van-cell :value="info.liaisonMan" title="紧急联系人" />

      <van-cell :value="info.liaisonManTel" title="手机号" />
    </div>
    <div class="card">
      <van-cell :value="info.familyDoctor" title="家庭医生" />

      <van-cell :value="info.familyDoctorTel" title="手机号" />
    </div>

    <div class="card">
      <van-cell :value="info.placeWork" title="工作单位" />

      <van-cell :value="info.post" title="职务" />
    </div>
    <div class="card" v-if="info.textarea">
      <van-cell :value="info.remarks" type="info.textarea" />
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";
export default {
  components: {},
  data() {
    return {
      info: {},
    };
  },
  computed: {},
  watch: {},
  created() {
    this.info = JSON.parse(localStorage.getItem("info"))[
      this.$route.query.infoId
    ];
    console.log("info: ", this.info);
  },
  mounted() {},
  beforeCreate() {},
  beforeMount() {},
  beforeUpdate() {},
  updated() {},
  beforeDestroy() {},
  destroyed() {},
  activated() {},
  filters: {
    updateTimeFilter(date) {
      if (!date) {
        return "";
      }
      return dayjs(date).infoat("YYYY-MM-DD");
    },
    birthdayFilter(date) {
      return dayjs(date).infoat("YYYY-MM-DD");
    },
    residentTypeFilter(n) {
      console.log("residentTypeFilter-n: ", n);
      const type = ["", "业主", "常住", "租客"];
      return type[n];
    },
    housesTypeFilter(n) {
      const type = ["自住", "出租", "空置"];
      return type[n];
    },
    sexTypeFilter(n) {
      const type = ["", "男", "女"];
      return type[n];
    }
  },
  methods: {}
};
</script>
<style lang="scss" scoped>
.info {
  padding-top: 60px;
  .bt {
    .van-cell__title {
      box-sizing: border-box;
      padding-left: 10px;
      position: relative;
      &::after {
        content: "*";
        color: #f5222d;
        display: block;
        position: absolute;
        left: 0;
        top: 3px;
      }
    }
  }
}
.card {
  background: #fff;
  width: 94%;
  border-radius: 10px;
  padding: 10px 0;
  margin: 0 auto 10px auto;
}
.van-cell {
  .van-cell__title {
    text-align: left;
    display: block;
    width: 6.2em;
    flex: none;
    margin-right: 12px;
  }
  .van-cell__value {
    text-align: left;
  }
  .van-icon {
    position: absolute;
    right: 20px;
  }
}
//@import url(); 引入公共css类
</style>
