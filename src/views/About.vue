<template>
  <div class="about">
    <van-nav-bar title="登记列表" fixed />
    <div class="card">
      <van-swipe-cell v-for="(item, i) of info" :key="i">
        <van-cell-group class="box" @click="go(i)">
          <van-cell class="t" :value="addr(item)" />
          <van-cell class="m" :value="`登记时间：${item.upDateEdit}--${i}`" />
        </van-cell-group>
        <template #right>
          <van-button
            square
            text="删除"
            @click="del(i)"
            type="danger"
            class="delete-button"
          />
        </template>
      </van-swipe-cell>
    </div>
    <div class="btn" @click="goDj">居民登记</div>
  </div>
</template>

<script>
// import dayjs from "dayjs";
export default {
  components: {},
  data() {
    return {
      info: [],
    };
  },
  computed: {},
  watch: {},
  created() {
    this.info = JSON.parse(localStorage.getItem("info"));
  },
  mounted() {},
  beforeCreate() {},
  beforeMount() {},
  beforeUpdate() {},
  updated() {},
  beforeDestroy() {},
  destroyed() {},
  activated() {},
  methods: {
    goDj() {
      this.$router.push({ name: "Home" });
    },
    del(i) {
      console.log("i: ", i);
      // return
      // this.info = this.info.splice(i,1);

      this.info.splice(i, 1);
      console.log("info: ", this.info);
      localStorage.setItem("info", JSON.stringify(this.info));
    },
    go(i) {
      console.log("i: ", i);
      this.$router.push({ name: "Info", params: { infoId: i } });
    },
    addr(item) {
      console.log("item: ", item);
      return `${item.villageName} ${item.buildNum}号楼 ${item.numberPlate}号`;
    },
  },
};
</script>
<style>
.van-cell-group {
  padding: 20px 0;
}
.van-cell::after {
  border: none;
}
</style>
<style lang="scss">
::v-deep .box {
  padding: 10px 0;
}
.t {
  font-size: 16px;
  font-weight: bold;
  padding-bottom: 0;
}
.m {
  color: #aaa;
  .van-cell__value--alone {
    color: #aaa;
    text-align: left;
  }
}
.about {
  background: #f7f4f8;
  position: relative;
  .btn {
    position: fixed;
    height: 50px;
    width: 100%;
    bottom: 0;
    left: 0;
    background-image: linear-gradient(to right, #00e8db, #00ea7f);
    line-height: 50px;
    text-align: center;
    color: #fff;
  }
}
.card {
  background: #fff;
  width: 94%;
  border-radius: 10px;
  padding: 10px 0;
  margin: 60px auto 10px auto;
}
.goods-card {
  margin: 0;
  background-color: #fff;
}

.delete-button {
  height: 100%;
}
</style>
