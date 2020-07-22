<!--  -->
<template>
  <div class="page">
    <van-nav-bar title="居民登记" fixed left-arrow @click-left="onClickLeft" />
    <van-form @submit="onSubmit">
      <div class="card">
        <van-cell
          is-link
          class="bt"
          @click="showVillage = true"
          title="  所属小区"
          :value="form.villageName"
          :rules="[{ required: true, message: '请选择所属小区' }]"
        >
        </van-cell>

        <van-popup
          v-model="showVillage"
          position="bottom"
          style="height: 300px;"
        >
          <van-picker
            title="所属小区"
            show-toolbar
            :columns="columnsVillage"
            @confirm="onConfirmVillage"
            @cancle="showVillage = false"
          />
        </van-popup>

        <van-field
          v-model="form.buildNum"
          class="bt"
          label="楼栋号"
          placeholder="楼栋号"
          :rules="[{ required: true, message: '请输入楼栋号' }]"
        />

        <van-field
          v-model="form.numberPlate"
          class="bt"
          label="门牌号"
          placeholder="门牌号"
          :rules="[{ required: true, message: '请输入楼栋号' }]"
        />
        <div class="radio">
          <span class="label bt">房屋类型</span>
          <div class="radio-box">
            <van-radio-group v-model="form.housesType">
              <van-radio name="1">自住</van-radio>
              <van-radio name="2">出租</van-radio>
              <van-radio name="3">空置</van-radio>
            </van-radio-group>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="radio" style="border-bottom: 1px #f5f5f5 solid;">
          <span class="label bt">居民类型</span>
          <div class="radio-box">
            <van-radio-group v-model="form.residentType">
              <van-radio name="1">业主</van-radio>
              <van-radio name="2">常住</van-radio>
              <van-radio name="3">租客</van-radio>
            </van-radio-group>
          </div>
        </div>

        <van-field
          v-model="form.name"
          class="bt"
          name="姓名"
          label="姓名"
          placeholder="姓名"
          :rules="[{ required: true, message: '请输入姓名' }]"
        />
        <div class="radio" style="border-bottom: 1px #f5f5f5 solid;">
          <span class="label bt">性别</span>
          <div class="radio-box">
            <van-radio-group v-model="form.sexType">
              <van-radio name="1">男</van-radio>
              <van-radio name="2">女</van-radio>
            </van-radio-group>
          </div>
        </div>

        <van-cell
          is-link
          class="bt"
          @click="showPopupBirthday"
          title="出生日期"
          :value="birthdayFilter"
        ></van-cell>
        <van-popup
          v-model="showBirthday"
          position="bottom"
          style="height: 300px;"
        >
          <van-datetime-picker
            v-model="form.birthday"
            type="date"
            title="选择年月日"
            @confirm="onConfirmBirthday"
            @cancel="onCancelBirthday"
            :min-date="minDate"
            :max-date="maxDate"
          />
        </van-popup>

        <van-field
          v-model="form.identityCard"
          class="bt"
          name="身份证"
          label="身份证"
          placeholder="身份证"
          :rules="[{ validator: validatorSfz, message: '请输入有效身份证' }]"
        />

        <van-field
          v-model="form.tel"
          class="bt"
          type="tel"
          name="手机号"
          label="手机号"
          placeholder="手机号"
          :rules="[{ validator, message: '请输入正确手机号' }]"
        />

        <van-cell
          is-link
          @click="showPopupLevelOfEducation"
          class="bt"
          title="文化程度"
          :value="form.levelOfEducation"
        ></van-cell>
        <van-popup
          v-model="showLevelOfEducation"
          position="bottom"
          style="height: 300px;"
        >
          <van-picker
            title="文化程度"
            show-toolbar
            :columns="columnsLevelOfEducation"
            @confirm="onConfirmLevelOfEducation"
            @cancle="showLevelOfEducation = false"
          />
        </van-popup>

        <div class="radio">
          <span class="label bt">党员是否</span>
          <div class="radio-box">
            <van-radio-group v-model="form.isDy">
              <van-radio name="是">是</van-radio>
              <van-radio name="否">否</van-radio>
            </van-radio-group>
          </div>
        </div>
      </div>

      <div class="card">
        <van-cell
          is-link
          @click="showPopupRelation"
          class="bt"
          title="与户主关系"
          :value="form.relation"
        ></van-cell>

        <van-popup
          v-model="showRelation"
          position="bottom"
          style="height: 300px;"
        >
          <van-picker
            title="与户主关系"
            show-toolbar
            :columns="columnsRelation"
            @confirm="onConfirmRelation"
            @cancle="showRelation = false"
          />
        </van-popup>

        <van-field
          v-model="form.address"
          name="户口所在地"
          class="bt"
          label="户口所在地"
          placeholder="户口所在地"
          :rules="[{ required: true, message: '请输入户口所在地' }]"
        />

        <van-cell
          is-link
          @click="showPopupUpdateTime"
          title="户口变动日期"
          :value="updateTimeFilter"
        ></van-cell>

        <van-popup
          v-model="showUpdateTime"
          position="bottom"
          style="height: 300px;"
        >
          <van-datetime-picker
            v-model="form.updateTime"
            type="date"
            title="选择年月日"
            @confirm="onConfirmUpdateTime"
            @cancel="showUpdateTime = false"
            :min-date="minDate"
            :max-date="maxDate"
          />
        </van-popup>
      </div>

      <div class="card">
        <van-field v-model="tsTag" label="特殊标签" placeholder="特殊标签" />
        <div class="tag-box">
          <span class="tag" v-for="(item, i) in form.tag" :key="i"
            >{{ item }} <van-icon class="err" name="cross" @click="del(i)"
          /></span>
          <span style="clear: both"></span>
        </div>
      </div>

      <div class="card">
        <van-field
          v-model="form.liaisonMan"
          name="紧急联系人"
          label="紧急联系人"
          placeholder="紧急联系人"
        />

        <van-field
          v-model="form.liaisonManTel"
          type="tel"
          name="手机号"
          label="手机号"
          placeholder="手机号"
        />
      </div>
      <div class="card">
        <van-field
          v-model="form.familyDoctor"
          name="家庭医生"
          label="家庭医生"
          placeholder="家庭医生"
        />

        <van-field
          v-model="form.familyDoctorTel"
          type="tel"
          name="手机号"
          label="手机号"
          placeholder="手机号"
        />
      </div>

      <div class="card">
        <van-field
          v-model="form.placeWork"
          name="工作单位"
          label="工作单位"
          placeholder="工作单位"
        />

        <van-field
          v-model="form.post"
          type="form.tel"
          name="职务"
          label="职务"
          placeholder="职务"
        />
      </div>
      <div class="card">
        <van-field
          v-model="form.remarks"
          rows="2"
          type="textarea"
          name="备注信息"
          maxlength="1000"
          placeholder="备注信息"
          show-word-limit
        />
      </div>
      <div style="width: 100%; height: 60px;"></div>
      <van-button class="btn" round block type="info" native-type="submit">
        保存
      </van-button>

      <!-- <div class="btn" native-type="submit">保存</div> -->
    </van-form>
  </div>
</template>

<script>
import dayjs from "dayjs";
export default {
  props: {
    formId: {
      type: Number,
    },
  },
  components: {},
  data() {
    return {
      infoId: "",
      tsTag: "",
      isEdit: false,
      minDate: new Date(1949, 0, 1),
      maxDate: new Date(2025, 10, 1),
      currentDate: new Date(),
      showBirthday: false,
      showLevelOfEducation: false,
      showUpdateTime: false,
      showRelation: false,
      showVillage: false,
      columnsVillage: ["虹旭小区", "虹旭二小区", "虹警新苑", "中星雅苑"],
      columnsLevelOfEducation: ["高中", "大专", "本科", "硕士", "博士"],
      columnsRelation: ["夫妻", "父子", "父女", "母子", "母女", "其他"],
      industry_list: [
        {
          parent_ind: "女装",
          name: "连衣裙",
        },
        {
          name: "女装",
        },
        {
          parent_ind: "女装",
          name: "半身裙",
        },
        {
          parent_ind: "女装",
          name: "A字裙",
        },
        {
          name: "数码",
        },
        {
          parent_ind: "数码",
          name: "电脑配件",
        },
        {
          parent_ind: "电脑配件",
          name: "内存",
        },
      ],
      form: {
        buildNum: "",
        numberPlate: "",
        name: "",
        identityCard: "",
        relation: "",
        tel: "",
        birthday: dayjs("2020-1-1"),
        updateTime: null,
        levelOfEducation: "",
        housesType: "1",
        residentType: "1",
        address: "",
        isDy: "是",
        liaisonMan: "",
        liaisonManTel: "",
        familyDoctor: "",
        familyDoctorTel: "",
        placeWork: "",
        post: "",
        tag: [],
        remarks: "",
        sexType: "1",
        upDateEdit: null,
        villageName: "",
      },
    };
  },
  computed: {
    birthdayFilter() {
      if (!this.form.birthday) {
        return "";
      }
      return dayjs(this.form.birthday).format("YYYY-MM-DD");
    },
    updateTimeFilter() {
      if (!this.form.updateTime) {
        return "";
      }
      return dayjs(this.form.updateTime).format("YYYY-MM-DD");
    },
  },
  watch: {},
  created() {
    this.get()
    if (this.$route.query.infoId !== undefined) {
      this.isEdit = true;
      this.infoId = this.$route.query.infoId;
      this.form = JSON.parse(localStorage.getItem("info"))[this.infoId];
      console.log("form: ", this.form);
    }
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
    get() {
      let o = {}
      this.industry_list.forEach(item => {
        if (item.parent_ind === undefined) {
          o[item.name] = {}
        }
      });
      console.log('o: ', o)
    },
    del(i) {
      this.form.tag.splice(i, 1);
    },
    onClickLeft() {
      this.$router.go(-1);
    },
    validatorSfz(val) {
      return (
        /^[1-9][0-9]{5}(19|20)[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|31)|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}([0-9]|x|X)$/.test(
          val
        ) && val !== ""
      );
    },
    validator(val) {
      return /^1(3|4|5|7|8)\d{9}$/.test(val) && val !== "";
    },
    onConfirmLevelOfEducation(e) {
      console.log("onConfirmLevelOfEducation-e: ", e);
      this.form.levelOfEducation = e;
      this.showLevelOfEducation = false;
    },
    onSubmit() {
      console.log("formId: ", this.formId);
      console.log("form: ", this.form);
      const param = this.form;
      param.formId = this.formId;
      if (this.tsTag) {
        param.tag.push(this.tsTag);
      }
      param.upDateEdit = dayjs(new Date()).format("YYYY-MM-DD HH:mm:ss");
      console.log("param: ", param);
      let info = localStorage.getItem("info");
      if (this.isEdit) {
        info = JSON.parse(info);
        info[this.infoId] = param;
      } else {
        if (info) {
          info = JSON.parse(info);
          info.push(param);
        } else {
          info = [];
          info.push(param);
        }
      }
      localStorage.setItem("info", JSON.stringify(info));
      this.$router.push({ name: "About" });
    },
    showPopupUpdateTime() {
      this.showUpdateTime = true;
    },
    showPopupRelation() {
      this.showRelation = true;
    },
    showPopupBirthday() {
      this.showBirthday = true;
    },
    showPopupLevelOfEducation() {
      this.showLevelOfEducation = true;
    },
    onConfirmVillage(e) {
      this.form.villageName = e;
      this.showVillage = false;
    },
    onConfirmRelation(e) {
      console.log("e: ", e);
      this.showRelation = false;
      this.form.relation = e;
    },
    onCancelBirthday() {
      this.showBirthday = false;
    },
    onConfirmBirthday(e) {
      console.log("e: ", e);
      this.form.birthday = e;
      console.log("this.form.birthday: ", this.form.birthday);
      this.showBirthday = false;
    },
    onConfirmUpdateTime(e) {
      console.log("e: ", e);
      this.form.updateTime = e;
      console.log("this.form.updateTime: ", this.form.updateTime);
      this.showUpdateTime = false;
    },
    onChangeRelation(e) {
      console.log("e: ", e);
    },
  },
};
</script>
<style>
.radio .radio-box .van-radio-group .van-radio[data-v-902d4c44] {
  padding-right: 10px;
  font-size: 12px !important;
}
.card .van-cell::after {
  border: none !important;
}
.van-nav-bar i.van-icon {
  color: #000;
}
.btn.van-button--round {
  border-radius: 0;
  background-image: linear-gradient(to right, #00e8db, #00ea7f);
  border: none;
}
.van-radio__icon i.van-icon {
  border: 1px solid #c8c9cc;
}
.van-radio__icon--checked i.van-icon-success {
  background-color: #00e8db;
  border-color: #00e8db !important;
}
.bt .van-field__label {
  padding-left: 10px;
  position: relative;
}
span.bt {
  padding-left: 10px;
  position: relative;
}
.bt .van-field__label::after,
.bt::after {
  content: "*";
  color: #f5222d;
  display: block;
  position: absolute;
  left: 0;
  top: 3px;
}
</style>
<style scoped lang="scss">
::v-deep .van-radio__icon--checked .van-icon.van-radio__icon--checked {
  color: #fff;
  background-color: #00e8db !important;
  border-color: #00e8db !important;
}
.page {
  background: #f7f4f8;
  padding-top: 60px;
  position: relative;
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
  margin: 0 auto 10px auto;
  .tag-box {
    padding: 10px 16px;
    display: flex;
    flex-wrap: wrap;
  }
  .tag {
    display: inline-block;
    padding: 0 10px;
    height: 30px;
    line-height: 30px;
    font-size: 12px;
    background: #f3f3f3;
    border-radius: 4px;
    text-align: center;
    margin: 0 10px 10px 0;
    float: left;
    .err {
      margin-left: 10px;
    }
  }
  .van-cell {
    border-bottom: 1px #f5f5f5 solid;
    &:last-child {
      border-bottom: none;
    }
  }
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
.radio {
  display: flex;
  padding: 10px 16px;
  .label {
    width: 6.2em;
    margin-right: 12px;
    font-size: 14px;
    text-align: left;
    box-sizing: border-box;
  }
  .radio-box {
    flex: 1;
    display: flex;
    padding-right: 20px;
    .van-radio-group {
      width: 100%;
      display: flex;
      justify-content: left;
      .van-radio {
        padding-right: 10px;
        font-size: 14px;
      }
    }
  }
}
</style>
