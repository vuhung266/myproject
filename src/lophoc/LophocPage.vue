<template>
<v-layout>
<p>
            <router-link to="/login">Logout</router-link>
            <router-link to="/">Index</router-link>
</p>
      
  <v-flex class="text-center">
        <div class="d-block"> 
        <v-breadcrumbs :items="itemsbreadcrumbs">
        <template v-slot:divider>
            <v-icon>mdi-chevron-right</v-icon>
        </template>
        </v-breadcrumbs>
        </div>

      
    
  <v-card>
    <v-toolbar
      color="indigo"
      dark
    >
      <v-toolbar-title>Danh sách lớp</v-toolbar-title>
      
      <v-spacer></v-spacer>
      <v-dialog v-model="dialog" persistent max-width="600px">
        
      <template v-slot:activator="{ on }">
        <v-btn color="primary" dark v-on="on">
          <v-icon dark>mdi-plus</v-icon>
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="headline">Thêm lớp học</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field label="Tên lớp" required></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field label="Số buổi dạy" required></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-menu
                  v-model="menu2"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="290px"
                >
                  <template v-slot:activator="{ on }">
                    <v-text-field
                      v-model="date"
                      label="Ngày khai giảng"
                      prepend-icon=""
                      readonly
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker v-model="date" @input="menu2 = false"></v-date-picker>
                </v-menu>
              </v-col>
              <v-col cols="12">
                <v-autocomplete
                  :items="['Skiing', 'Ice hockey', 'Soccer', 'Basketball', 'Hockey', 'Reading', 'Writing', 'Coding', 'Basejump']"
                  label="Giảng viên"
                  multiple
                ></v-autocomplete>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialog = false">Đóng</v-btn>
          <v-btn color="blue darken-1" text @click="dialog = false">Lưu</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
      
    </v-toolbar>
    <v-tabs
      v-model="tab"
      background-color="grey darken-4"
      centered
    >
      <v-tabs-slider>aa</v-tabs-slider>

      <v-tab href="#tab-1" @click="danhsachlophoc('SC1')">
        Sơ cáp 1
      </v-tab>

      <v-tab href="#tab-2" @click="danhsachlophoc('SC2')">
        Sơ cáp 2
      </v-tab>

      <v-tab href="#tab-3" @click="danhsachlophoc('XD')">
        Xã Đàn
      </v-tab>

      <v-tab href="#tab-4" @click="danhsachlophoc('LuyenThiDH')">
        Luyện thi ĐH
      </v-tab>

      <v-tab href="#tab-5" @click="danhsachlophoc('KhauNgu')">
        Khẩu ngữ
      </v-tab>

      <v-tab href="#tab-6" @click="danhsachlophoc('HSK6')">
        HSK6
      </v-tab>
      <v-tab href="#tab-7" @click="danhsachlophoc('HSK5')">
        HSK5
      </v-tab>
      <v-tab href="#tab-8" @click="danhsachlophoc('HSK4')">
        HSK4
      </v-tab>
      <v-tab href="#tab-9" @click="danhsachlophoc('Boya3')">
        Boya3
      </v-tab>
      <v-tab href="#tab-10" @click="danhsachlophoc('Boya2')">
        Boya2
      </v-tab>
      <v-tab href="#tab-11" @click="danhsachlophoc('Boya1')">
        Boya1
      </v-tab>
    </v-tabs>
    <v-tabs-items v-model="tab">
      <v-tab-item
        v-for="i in 11"
        :key="i"
        :value="'tab-' + i"
      >
      <div class="container">
        <v-layout wrap >
        <v-card flat v-for="lophoc in dslophoc" :key="lophoc.id" class="mx-auto mb-5 mt-5 "  max-width="320"  outlined>
          <v-card-text @click="xemchitietlophoc(lophoc.id);"><h4 href="#1" >{{ lophoc.name }}</h4></v-card-text>
          <div class="grey darken-4">
          <v-card-subtitle class="pb-0">Khai giảng: {{ lophoc.ngaykhaigiang }}</v-card-subtitle>
          <v-card-subtitle class="small">Số buổi dạy: {{ lophoc.sobuoiday }}</v-card-subtitle>
          </div>
          <v-card-actions class="mt-4">
            <v-btn small outlined >Điểm danh</v-btn>
            <v-btn small>Nhập điểm</v-btn>
            <v-btn small>Bài tập</v-btn>
          </v-card-actions>
        </v-card>
        </v-layout>
      </div>
      </v-tab-item>
    </v-tabs-items>
  </v-card>
  </v-flex>
</v-layout>
</template>
<script>
import axios from 'axios';
var link = 'https://restapi.quiz.edu.vn/trungtamhoaiphuong/lophoc';
export default {
    head: {
      title: "Danh sách lớp"
    },
    data () {
      return {
        dialog: false,
         itemsbreadcrumbs: [
        {
          text: 'Trang chủ',
          disabled: false,
          href: 'breadcrumbs_dashboard',
        },
        {
          text: 'Lớp học',
          disabled: false,
          href: 'lophoc',
        },
      ],
        errors: [],
        dslophoc: [],
        tab: null,
        date: new Date().toISOString().substr(0, 10),
        menu: false,
        modal: false,
        menu2: false,
      }
    },
    created: function () {
      this.danhsachlophoc('SC1');
    },
    methods: {
      danhsachlophoc: function(classtype){
        axios.get(link+'/'+classtype)
        .then(response => {
          this.dslophoc = response.data;
          console.log(dslophoc);
        })
        .catch(e => {
          this.errors.push(e)
        })
      },
      xemchitietlophoc: function(e){
        this.$router.push({name: 'chi-tiet-lop-hoc' ,  params: {classid:e}})
      },
    }
  }
</script>
<style >
.abc {height:100px}
</style>