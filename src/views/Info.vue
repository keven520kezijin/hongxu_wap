<!--  -->
<template>
  <div class="">
    <van-cell
      title="楼栋号"
      :value="info.buildNum"
    ></van-cell>
    <van-cell
      title="门牌号"
      :value="info.numberPlate"
    ></van-cell>
    <van-cell
      title="房屋类型"
      :value="info.housesType | housesTypeFilter"
    ></van-cell>
    <van-cell
      title="居民类型"
      :value="info.residentType | residentTypeFilter"
    ></van-cell>
    <van-cell
      title="性别"
      :value="info.sexType | sexTypeFilter"
    ></van-cell>
    <van-cell
      title="出生日期"
      :value="info.birthday | birthdayFilter"
    ></van-cell>
    <van-cell
      title="身份证"
      :value="info.identityCard"
    ></van-cell>
    <van-cell
      title="手机号"
      :value="info.tel"
    ></van-cell>
    <van-cell
      title="文化程度"
      :value="info.levelOfEducation"
    ></van-cell>
    <van-cell
      title="与户主关系"
      :value="info.relation"
    ></van-cell>
    <van-cell
      title="户口所在地"
      :value="info.address"
    ></van-cell>
    <van-cell
      title="户口变动日期"
      :value="info.updateTime | updateTimeFilter"
    ></van-cell>
    <van-cell
      title="紧急联系人"
      :value="info.liaisonMan"
    ></van-cell>
    <van-cell
      title="家庭医生"
      :value="info.familyDoctor"
    ></van-cell>
    <van-cell
      title="工作单位"
      :value="info.placeWork"
    ></van-cell>
    <van-cell
      title="职务"
      :value="info.post"
    ></van-cell>
    <van-cell
      title="备注信息"
      :value="info.remarks"
    ></van-cell>
  </div>
</template>

<script>

import dayjs from "dayjs";
export default {
  components: {},
  data() {
    return {
      info: {}
    };
  },
  computed: {},
  watch: {},
  created() {
    this.info = JSON.parse(localStorage.getItem('info'))[this.$route.params.infoId]
    console.log('info: ', this.info)
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
        return ''
      }
      return dayjs(date).format("YYYY-MM-DD")
    },
    birthdayFilter(date) {
      return dayjs(date).format("YYYY-MM-DD")
    },
    residentTypeFilter(n) {
      console.log('residentTypeFilter-n: ', n)
      const type = ['', '业主', '常住', '租客']
      return type[n]
    },
    housesTypeFilter(n) {
      const type = ['自住', '出租', '空置']
      return type[n]
    },
    sexTypeFilter(n) {
      const type = ['', '男', '女']
      return type[n]
    }
  },
  methods: {},
};
</script>
<style lang="scss" scoped>
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
