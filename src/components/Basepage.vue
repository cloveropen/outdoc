<template>
  <div>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app
    >
      <v-card>
        <v-card-text>
          <div class="text--primary"><b>候诊患者</b></div>
          <div class="flex-grow-1"></div>
          <v-text-field
            v-model="search"
            append-icon="search"
            label="查询(门诊号/姓名)"
            single-line
            hide-details
          ></v-text-field>
        </v-card-text>
        <v-data-table
          :items-per-page="5"
          single-select
          :headers="headers"
          :items="desserts"
          :search="search"
        >
          <template v-slot:item.name="{ item }">
            <v-icon small class="mr-2" @click="pidChanged(item)">
              {{ item.name }}
            </v-icon>
          </template>
        </v-data-table>
      </v-card>
      <v-card>
        <v-card-text>
          <div class="text--primary"><b>就诊患者者</b></div>
          <div class="flex-grow-1"></div>
        </v-card-text>
        <v-data-table
          dense
          hide-default-footer
          :headers="headers"
          :items="desserts"
          :search="search"
        ></v-data-table>
      </v-card>
      <v-card>
        <v-card-text>
          <div class="text--primary"><b>已诊患者</b></div>
          <div class="flex-grow-1"></div>
          <v-text-field
            v-model="search"
            append-icon="search"
            label="查询(门诊号/姓名)"
            single-line
            hide-details
          ></v-text-field>
        </v-card-text>
        <v-data-table
          dense
          :items-per-page="50"
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
      dark
    >
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
          v-on="on"
        >
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
    search: "",
    headers: [
      {
        text: "姓名",
        align: "left",
        sortable: true,
        value: "name",
        width: 100
      },
      { text: "挂号单", value: "calories", width: 120 },
      { text: "门诊号", value: "fat", width: 120 },
      { text: "类型", value: "carbs", width: 80 }
    ],
    desserts: [
      {
        name: "猪八戒",
        calories: 159,
        fat: 123456,
        carbs: "候诊"
      },
      {
        name: "唐三藏",
        calories: 237,
        fat: 123457,
        carbs: "转诊"
      },
      {
        name: "沙和尚",
        calories: 262,
        fat: 123458,
        carbs: "预约"
      }
    ]
  }),
  methods: {
    
    pidChanged(e) {
      //console.log("e=" + e.target.innerText);
      window.alert("e=" + JSON.stringify(e));
      this.$router.push({ name: "recept", params: { patient: e } });
    },    
    selectSource() {
      window.location.href = "http://www.cloveropen.com";
    }
  }
};
</script>
