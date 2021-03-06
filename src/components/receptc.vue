<template>
  <v-container class="grey lighten-5">
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-card class="mx-auto" max-width="99%" min-width="100%">
        <v-img
          class="white--text"
          height="60px"
          :src="require('../assets/outreg.jpg')"
        >
          <v-card-title class="align-end fill-height">门诊医生</v-card-title>
        </v-img>
        <v-card-text>
          <v-layout row wrap>
            &emsp;&emsp;
            <v-flex d-flex>
              <v-text-field v-model="out_reg.exPid" label="就诊科室" readonly>
              </v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.patientName"
                label="患者姓名"
                readonly
              ></v-text-field>
            </v-flex>
            <v-flex d-flex>
              <v-menu
                ref="menu"
                v-model="menu"
                :close-on-content-click="false"
                transition="scale-transition"
                offset-y
                full-width
                min-width="290px"
              >
                <template v-slot:activator="{ on }">
                  <v-text-field
                    v-model="date"
                    label="出生日期"
                    prepend-icon="event"
                    readonly
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker
                  ref="picker"
                  v-model="date"
                  :max="new Date().toISOString().substr(0, 10)"
                  min="1910-01-01"
                  @change="save"
                  locale="zh-cn"
                ></v-date-picker>
              </v-menu>
            </v-flex>
            <v-flex d-flex>
              <v-text-field
                v-model="out_reg.pid"
                label="门诊号"
                disabled
              ></v-text-field>
            </v-flex>
            <v-flex>
              <v-layout row wrap>
                <v-flex d-flex>
                  &emsp;&emsp;
                  <v-text-field
                    v-model="out_reg.ageY"
                    label="年龄(岁)"
                  ></v-text-field>
                </v-flex>
                <v-flex d-flex>
                  &emsp;&emsp;
                  <v-text-field
                    v-model="out_reg.ageM"
                    label="年龄(月)"
                  ></v-text-field>
                </v-flex>
                <v-flex d-flex>
                  &emsp;&emsp;
                  <v-text-field
                    v-model="out_reg.ageD"
                    label="年龄(天)"
                  ></v-text-field
                  >&emsp;&emsp;
                </v-flex>
              </v-layout>
            </v-flex>

            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.gender"
                :items="genders"
                item-text="item-text"
                item-value="item-value"
                label="职业"
                hide-details
                prepend-icon="map"
                required
              ></v-select>
            </v-flex>
          </v-layout>
          <v-layout row wrap>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.idcard"
                label="身份证号"
              ></v-text-field
              >&emsp;&emsp;
            </v-flex>
            <v-flex d-flex>
              <v-dialog
                ref="dialog"
                v-model="modal"
                :return-value.sync="date"
                persistent
                full-width
                width="290px"
              >
                <template v-slot:activator="{ on }">
                  <v-text-field
                    v-model="date1"
                    label="发病日期"
                    prepend-icon="event"
                    readonly
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="date" locale="zh-cn" scrollable>
                  <div class="flex-grow-1"></div>
                  <v-btn text color="primary" @click="modal = false"
                    >Cancel</v-btn
                  >
                  <v-btn text color="primary" @click="$refs.dialog.save(date1)"
                    >OK</v-btn
                  >
                </v-date-picker>
              </v-dialog>
            </v-flex>

            <v-flex d-flex>
              <v-dialog
                ref="dialog"
                v-model="modal2"
                :return-value.sync="time"
                persistent
                full-width
                width="290px"
              >
                <template v-slot:activator="{ on }">
                  <v-text-field
                    v-model="time"
                    label="发病时间"
                    prepend-icon="access_time"
                    readonly
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-time-picker v-if="modal2" v-model="time" full-width>
                  <div class="flex-grow-1"></div>
                  <v-btn text color="primary" @click="modal2 = false"
                    >取消</v-btn
                  >
                  <v-btn text color="primary" @click="$refs.dialog.save(time)"
                    >确认</v-btn
                  >
                </v-time-picker>
              </v-dialog>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-switch v-model="out_reg.visitPriority" label="复诊"></v-switch>
            </v-flex>
          </v-layout>

          <v-layout row wrap>
            <v-flex d-flex flex-grow-1>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.regType"
                label="单位"
                required
                :items="reg_types"
                item-text="item-text"
                item-value="item-value"
                hide-details
                prepend-icon="map"
                single-line
                @input="reg_typeChanged($event)"
              ></v-select>
            </v-flex>
            <v-flex d-flex flex-shrink-0>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.idcard"
                label="单位电话"
              ></v-text-field
              >&emsp;&emsp;
            </v-flex>
          </v-layout>
          <v-layout row wrap>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.addrProv"
                :items="addr_provs"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '单位或住址省份不能为空']"
                label="住址(省份)"
                hide-details
                prepend-icon="group_work"
                required
                @input="prov_Changed"
              ></v-select>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.addrCity"
                :items="addr_citys"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '单位或住址-市不能为空']"
                label="住址(市)"
                hide-details
                prepend-icon="group_work"
                required
                @input="city_Changed"
              ></v-select>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.addrCounty"
                :items="addr_countys"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '单位或住址-区县不能为空']"
                label="住址(区县)"
                prepend-icon="group_work"
                required
                @input="county_Changed"
              ></v-select
              >&emsp;&emsp;
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.addrTownship"
                :items="addr_townships"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '单位或住址-街道社区不能为空']"
                label="住址(街道社区)"
                hide-details
                prepend-icon="group_work"
                required
                hint="住址(街道社区)"
                persistent-hint
              ></v-select
              >&emsp;&emsp;
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.addrHouseNmb"
                label="住址(详细地址)"
              ></v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.addrHouseNmb"
                label="监护人"
              ></v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.addrHouseNmb"
                label="联系电话"
              ></v-text-field>
            </v-flex>
          </v-layout>
          <!-- <v-spacer></v-spacer>           -->
          <v-layout row wrap>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.addrHouseNmb"
                label="过敏情况"
              ></v-text-field>
            </v-flex>
          </v-layout>
          <v-layout row wrap>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-textarea
                class="mx-2"
                rows="1"
                auto-grow
                prepend-icon="comment"
                v-model="out_reg.addrHouseNmb"
                label="摘要"
              ></v-textarea>
            </v-flex>
          </v-layout>
 <v-layout row wrap>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.addrHouseNmb"
                label="患者费别"
                readonly
              ></v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.addrHouseNmb"
                label="挂号类别"
                readonly
              ></v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.addrHouseNmb"
                label="健康卡号"
                readonly
              ></v-text-field>
            </v-flex>
          </v-layout>

          <v-row>
            <v-col sm="12">
              <!-- <v-card class="pa-2" outlined tile>就诊人信息</v-card> -->
              <v-row no-gutters>
                <v-col cols="6" sm="7">
                  <v-card class="pa-4" elevation="18">
                    <div>
                      <video
                        ref="video"
                        id="video"
                        width="640"
                        height="480"
                        autoplay
                      ></video>
                    </div>
                  </v-card>
                </v-col>
                <v-col cols="6" sm="5">
                  <v-card class="pa-4" elevation="18" tile>
                    <v-row>
                      <v-col cols="12">
                        <div class="title">患者医保信息</div>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="6">
                        <div class="subtitle-1">
                          &emsp;&emsp;医保个人编号:{{ out_reg.micard }}
                        </div>
                      </v-col>
                      <v-col cols="6">
                        <div class="subtitle-1">
                          &emsp;&emsp;医保卡余额:{{ out_reg.miPaccLeft }}
                        </div>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="6">
                        <div class="subtitle-1">
                          &emsp;&emsp;医保类别:{{ out_reg.miType }}
                        </div>
                      </v-col>
                      <v-col cols="6">
                        <div class="subtitle-1">
                          &emsp;&emsp;参保单位:{{ out_reg.miCompany }}
                        </div>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="12">
                        <div class="subtitle-1">
                          &emsp;&emsp;读卡信息:{{ out_reg.miStr }}
                        </div>
                      </v-col>
                    </v-row>
                  </v-card>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-card-text>

        <v-card-actions class="justify-center">
          <v-btn id="snap" :disabled="!valid" color="success" @click="capture"
            >拍照</v-btn
          >         
          <v-btn
            :disabled="!valid"
            color="success"
            v-on:click="readcardClicked($event)"
            >接诊</v-btn
          >
          <v-btn
            :disabled="!valid"
            color="success"
            @click="outregcashClicked($event)"
            >下医嘱</v-btn
          >
          <v-btn
            :disabled="!valid"
            color="success"
            @click="outregweixinClicked($event)"
            >写病历</v-btn
          >                   
        </v-card-actions>
      </v-card>
    </v-form>
    <canvas ref="canvas" id="canvas" width="640" height="480" hidden></canvas>
    <!-- <ul>
      <v-list v-for="c in captures" :key="c">
        <img v-bind:src="c" height="180" />
      </v-list>
    </ul> -->

    <v-row>
      <v-col sm="12">
        <!--  第二级 -->
        <v-row no-gutters>
          <v-col cols="8" sm="6">
            <!-- 第三级 显示照片 -->
            <v-row no-gutters>
              <v-col
                ><img v-bind:src="this.out_reg_pic.pic1" height="120"
              /></v-col>
              <v-col
                ><img v-bind:src="this.out_reg_pic.pic2" height="120"
              /></v-col>
              <v-col
                ><img v-bind:src="this.out_reg_pic.pic3" height="120"
              /></v-col>
            </v-row>
            <v-row no-gutters>
              <v-col
                ><img v-bind:src="this.out_reg_pic.pic4" height="120"
              /></v-col>
              <v-col
                ><img v-bind:src="this.out_reg_pic.pic5" height="120"
              /></v-col>
              <v-col
                ><img v-bind:src="this.out_reg_pic.pic6" height="120"
              /></v-col>
            </v-row>
          </v-col>
          <v-col cols="4" sm="6">
            <v-card
              class="pa-2"
              outlined
              style="background-color: lightgrey;"
              tile
            >
              挂号单打印样式
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import {
  getpatient_type,
  get_regopcode,
  getgender,
  getid_type,
  getreg_type,
  getdept_codes,
  getdoctor_codes,
  getprovs,
  getcitys,
  getcountys,
  getstreets,
  getpatient,
  readcard_mi,
  outreg_cash,
  outreg_weixin,
  outreg_pic,
  sch_weixin,
  getregprice
} from "../scripts/outreg.js";

export default {
  data: () => ({
    valid: true,
    date: null,
    date1: null,
    time: null,
    menu: false,
    modal: false,
    modal2: false,
    nameRules: [
      v => !!v || "姓名不能为空",
      v => (v && v.length >= 2) || "姓名长度不能少于2个汉字"
    ],
    //barcodeRules: [
    //  v => !!v || "条形码不能为空",
    //  v => (v && v.length >= 13) || "条形码应该为13位数字"
    //],
    patient_types: [], //患者类别列表
    genders: [], //性别列表
    idcard_types: [], //身份证件类型列表
    dept_codes: [], //就诊科室列表
    doctor_codes: [], //专家号专家列表
    reg_types: [], //挂号类别
    addr_provs: [], //单位或住址(省份)
    addr_citys: [], //单位或住址(市)
    addr_countys: [], //单位或住址(区县)
    addr_townships: [], //单位或住址(街道)
    out_reg: {
      hspCode: process.env.VUE_APP_HSP_CODE,
      pid: "", //门诊号
      pidType: "O", //患者标识类别
      exPid: "", //条形码
      patientName: "", //患者姓名
      gender: "", //性别代码
      ageY: "", // 年龄
      ageM: "", //  年龄
      ageD: "", //  年龄
      patientType: "", //患者类型
      regType: "pz", // 挂号类别
      regPrice: 0.0, //挂号费
      checkPrice: 0.0, //诊察费
      visitPriority: "0", //就诊优先标志
      deptCode: "", //就诊科室
      doctorCode: "", //门诊接诊医生
      idcard: "", //患者身份证号码
      idcardType: "jmsfz", //患者身份证件类别
      addrProv: process.env.VUE_APP_HSP_PROV, //地址
      addrCity: process.env.VUE_APP_HSP_CITY,
      addrCounty: process.env.VUE_APP_HSP_COUNTY,
      addrTownship: "",
      addrStreet: "",
      addrHouseNmb: "",
      miPaccLeft: 0.0,
      miCompany: "",
      miStr: "",
      miType: "",
      micard: "",
      payType: "",
      regOpcode: "", //挂号员
      payCash: 0.0, //现金支付金额
      payPacc: 0.0, //医保(农合)个人帐户支付金额
      payFund: 0.0, //医保(农合)统筹支付金额
      payNfc: 0.0, //移动支付金额
      invoiceNmb: "", //挂号单收据流水号
      flowNmb: "", //挂号单操作员流水号
      mchIp: "", //本机局域网IP地址
      ver: process.env.VUE_APP_VERSION //版本号
    },
    video: {},
    canvas: {},
    //captures: [],
    capture_num: 0,
    out_reg_pic: {
      hspCode: process.env.VUE_APP_HSP_CODE,
      pid: "",
      exPid: "",
      patientName: "",
      idcard: "",
      healthId: "",
      micard: "",
      captureTime: "",
      captureOpid: "",
      mchIp: "",
      pic1: "",
      pic2: "",
      pic3: "",
      pic4: "",
      pic5: "",
      pic6: ""
    }
  }),
  created() {
    this.out_reg.regOpcode = get_regopcode();
    this.patient_types = getpatient_type();
    this.genders = getgender();
    this.idcard_types = getid_type();
    this.reg_types = getreg_type();
    this.dept_codes = getdept_codes();
    this.addr_provs = getprovs();
    this.addr_citys = getcitys(process.env.VUE_APP_HSP_PROV);
    this.addr_countys = getcountys(process.env.VUE_APP_HSP_CITY);
    this.addr_townships = getstreets(process.env.VUE_APP_HSP_COUNTY);
  },
  mounted() {
    // console.log("路由传参:"+JSON.stringify(this.$route.params.patient))
    this.video = this.$refs.video;
    if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
      navigator.mediaDevices.getUserMedia({ video: true }).then(stream => {
        this.video.srcObject = stream;
        this.video.play();
      });
    }
  },
  watch: {
    menu(val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = "YEAR"));
    }
  },
  methods: {
    save(date) {
      this.$refs.menu.save(date);
    },
    validate() {
      if (this.$refs.form.validate()) {
        this.snackbar = true;
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
    expidChanged(e) {
      let texpid = e;
      console.log("texpid e=" + e);
      if (texpid.length < 10) {
        return;
      }
      getpatient(texpid).then(data => {
        this.out_reg = data;
      });
    },
    readcardClicked(e) {
      console.log("e=" + e.target.innerText);
      readcard_mi();
    },
    outregcashClicked(e) {
      console.log("e=" + e.target.innerText);
      outreg_cash(this.out_reg).then(data => {
        this.out_reg.pid = data;
        console.log(
          "outregcashClicked this.out_reg_pic.pid=" + this.out_reg_pic.pid
        );
        this.out_reg_pic.pid = this.out_reg.pid;
        this.out_reg_pic.exPid = this.out_reg.exPid;
        this.out_reg_pic.patientName = this.out_reg.patientName;
        this.out_reg_pic.idcard = this.out_reg.idcard;
        this.out_reg_pic.healthId = this.out_reg.healthId;
        this.out_reg_pic.micard = this.out_reg.micard;
        this.out_reg_pic.captureOpid = this.out_reg.regOpcode;
        outreg_pic(this.out_reg_pic);
      });
    },
    outregweixinClicked(e) {
      console.log("e=" + e.target.innerText);
      outreg_weixin();
    },
    schweixinClicked(e) {
      console.log("e=" + e.target.innerText);
      sch_weixin();
    },
    dept_codeChanged(e) {
      let tdept_code = this.out_reg.deptCode;
      console.log("dept_codeChanged e=" + e);
      let tpost_tech = "1";
      this.doctor_codes = getdoctor_codes(tdept_code, tpost_tech);
    },
    reg_typeChanged(e) {
      console.log("reg_typeChanged e=" + e);
      let treg_type = this.out_reg.regType;
      getregprice(treg_type).then(data => {
        this.out_reg.regPrice = data[0];
        this.out_reg.checkPrice = data[1];
      });
    },
    //------------------获取指定省份的市列表---------------------------
    prov_Changed() {
      let tprovid = this.out_reg.addrProv;
      this.addr_citys = getcitys(tprovid);
    },
    //------------------获取指定市的区县列表---------------------------
    city_Changed() {
      let tcityid = this.out_reg.addrCity;
      this.addr_countys = getcountys(tcityid);
    },
    //------------------获取指定区县的街道列表---------------------------
    county_Changed() {
      let tcountyid = this.out_reg.addrCounty;
      this.addr_townships = getstreets(tcountyid);
    },
    capture() {
      this.canvas = this.$refs.canvas;
      var ctx = this.canvas.getContext("2d");
      //console.log(this.$refs.canvas.toDataURL("image/png"));
      ctx.drawImage(this.video, 0, 0, 640, 480);
      //this.captures.push(this.$refs.canvas.toDataURL("image/png"));
      switch (this.capture_num) {
        case 0:
          this.out_reg_pic.pic1 = this.$refs.canvas.toDataURL("image/png");
          this.capture_num++;
          break;
        case 1:
          this.out_reg_pic.pic2 = this.$refs.canvas.toDataURL("image/png");
          this.capture_num++;
          break;
        case 2:
          this.out_reg_pic.pic3 = this.$refs.canvas.toDataURL("image/png");
          this.capture_num++;
          break;
        case 3:
          this.out_reg_pic.pic4 = this.$refs.canvas.toDataURL("image/png");
          this.capture_num++;
          break;
        case 4:
          this.out_reg_pic.pic5 = this.$refs.canvas.toDataURL("image/png");
          this.capture_num++;
          break;
        case 5:
          this.out_reg_pic.pic6 = this.$refs.canvas.toDataURL("image/png");
          this.capture_num++;
          break;
        default:
          this.capture_num = 0;
          this.out_reg_pic.pic1 = this.$refs.canvas.toDataURL("image/png");
      }
      //--
    }
    // ---------------------end methods----------------
  }
};
</script>

<style></style>
