<template>
    <div>
        <section>
       <li @click="clickCity">
        <span>可补换的签发城市</span>
        <span>{{info.city.join(' ')}}</span>
      </li>
    </section>
    <section>
      <van-popup v-model="showCity" position="bottom" :overlay="true">
        <van-picker :columns="cityArray" @change="cityChange" ref="cityPicker" @cancel="onCancel" show-toolbar title="请选择签发城市" @confirm="cityConfirm"/>
      </van-popup>
    </section>
    </div>
</template>
<script>
import {cityList, costList} from '@/api/index';
import { Swipe, SwipeItem } from 'vant';
import Vue from "vue";
Vue.use(Swipe).use(SwipeItem);
export default {
  data(){
    return {
      showType: false,
      showCity: false,
      typeArray: ["补驾照", "换驾照"],
      // 签发城市
      cityList: [],
      cityArray: [],
      info: {
        type: '',
        city: []
      }
    }
  },
  created() {
    this.getCityList();
  },
  methods: {
    async getCityList(){
      let res = await cityList();
      res.data.forEach(item=>{
        item.list.forEach(value=>{
          delete value.list;
        })
      })
      console.log('res...', res);
      this.cityList = res.data;

      this.cityArray = [{
        values: this.cityList.map(item=>item.name)
      }, {
        values: this.cityList[0].list.map(item=>item.name)
      }]
    },
    async getCostList(){

    },
    cityChange(picker, values){
      let index = this.cityList.findIndex(item=>item.name == values[0]);
      this.cityArray[1].values = this.cityList[index].list.map(item=>item.name)
      // console.log('picker...', picker, values, this.cityArray, this.$refs.cityPicker, this.cityList[index].list.map(item=>item.name));
      this.$refs.cityPicker.setColumnValues(1,  this.cityList[index].list.map(item=>item.name))
    },
    cityConfirm(values){
      this.info.city = values;
      this.showCity = false;
    },
    onCancel(e){
      this.showType = false;
    },
    onConfirm(value){
      console.log('value...', value);
      this.info.type = value;
      this.onCancel();
    },
    clickType(){
      this.showType = true;
    },
    clickCity(){
      this.showCity = true;
    }
  }
}
</script>