<template>
  <div>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app>
     <v-card>
    <v-card-title>
      候诊患者
      <div class="flex-grow-1"></div>
      <v-text-field
        v-model="search"
        append-icon="search"
        label="查询(门诊号/姓名)"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table :items-per-page="5" single-select
      :headers="headers"
      :items="desserts"
      :search="search"
    > 
      <template v-slot:item.name="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="pidChanged(item)"
      >
       {{item.name}}
      </v-icon>     
      </template>
      
    </v-data-table>
  </v-card>
  <v-card>
  <v-card-title>
      就诊患者
      <div class="flex-grow-1"></div>     
    </v-card-title>
    <v-data-table  dense
    hide-default-footer
      :headers="headers"
      :items="desserts"
      :search="search"
      
    ></v-data-table>
  </v-card>
  <v-card>
  <v-card-title>
      已诊患者
      <div class="flex-grow-1"></div>
      <v-text-field
        v-model="search"
        append-icon="search"
        label="查询(门诊号/姓名)"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table dense 
      :headers="headers"
      :items="desserts"
      :search="search"
    ></v-data-table>
  </v-card>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="blue darken-3"
      dark>
      <v-toolbar-title style="width: 300px" class="ml-0 pl-4">
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <span class="hidden-sm-and-down">门诊医生工作站</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-tooltip right>
        <template v-slot:activator="{ on }">
          <v-btn icon to="/about" v-on="on">
            <v-icon>thumb_up_alt</v-icon>
          </v-btn>
        </template>
        <span>帮助手册</span>
      </v-tooltip>
      <v-btn icon to="/">
        <v-icon>notifications</v-icon>
      </v-btn>
      <v-btn icon large @click="selectSource">
        <v-avatar size="32px" item>
          <v-img :src="require('../assets/logo.svg')" alt="苜蓿草科技"></v-img>
        </v-avatar>
      </v-btn>
    </v-app-bar>
    <!-- <v-container fluid fill-height>
      <v-layout justify-start>
        <v-img :src="require('../assets/main_bg.jpg')" />
      </v-layout>
    </v-container>-->
    <v-tooltip right>
      <template v-slot:activator="{ on }">
        <v-btn
          bottom
          color="pink"
          dark
          fab
          fixed
          right 
          @click="clickMenu('logout')" 
          v-on="on">
          <v-icon>add</v-icon>
        </v-btn>
      </template>
      <span>退出登录</span>
    </v-tooltip>

    
  </div>
  
</template>

<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    dialog: false,
    drawer: null,
    search: '',
        headers: [
          {
            text: '姓名',
            align: 'left',
            sortable: true,
            value: 'name',
            width: 100
          },
          { text: '挂号单', value: 'calories', width: 120 },
          { text: '门诊号', value: 'fat', width: 120 },
          { text: '类型', value: 'carbs', width: 80 }         
        ],
        desserts: [
          {
            name: '猪八戒',
            calories: 159,
            fat: 123456,
            carbs: '候诊'
          },
          {
            name: '唐三藏',
            calories: 237,
            fat: 123457,
            carbs: '转诊'
          },
          {
            name: '沙和尚',
            calories: 262,
            fat: 123458,
            carbs: '预约'
          }
        ]
  }),
  methods: {
    clickMenu(tstr) {
      console.log("点击=" + tstr);
      if (tstr === "logout") {
        localStorage.removeItem("user");
        this.$router.push({ path: "/login" });
      }
      switch (tstr)
      {
        case "out_reg":
          this.$router.push({ path: "/outreg" });
          break;

        case "out_cash":
          this.$router.push({ path: "/outcash" });
          break;
        
        case "out_chk":
          this.$router.push({ path: "/outchk" });
          break;

        case "out_receipt":
          this.$router.push({ path: "/outreceipt" });
          break;

        case "detail_reg":
          this.$router.push({ path: "/detailreg" });
          break;

        case "detail_cash":
          this.$router.push({ path: "/detailcash" });
          break;

        case "detail_chk":
          this.$router.push({ path: "/detailchk" });
          break;
        
        case "detail_undo":
          this.$router.push({ path: "/detailundo" });
          break;
          
        case "detail_op":
          this.$router.push({ path: "/detailop" });
          break; 
          
        case "mg_dict":
          this.$router.push({ path: "/mgdict" });
          break; 
        
        case "mg_analyse":
          this.$router.push({ path: "/mganalyse" });
          break; 
        
        case "mg_invoice":
          this.$router.push({ path: "/mginvoice" });
          break; 

        default:
          localStorage.removeItem("user");
          this.$router.push({ path: "/login" });
      }
    },
     pidChanged(e) {
      //console.log("e=" + e.target.innerText);
      window.alert("e="+JSON.stringify(e))
    },
    pidChanged1(e) {
      //console.log("e=" + e.target.innerText);
      window.alert("e1="+JSON.stringify(e))
    },
    selectSource() {
      window.location.href = "http://www.cloveropen.com";
    }
  }
};
</script>
