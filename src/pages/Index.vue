<template>
  <div>
    <header>
        <p>提交订单</p>
        <p>填写收货地址</p>
        <p>正在办理</p>
        <p>办理完成</p>
    </header>
    <Upload/>
    <TypePicker/>
    <section>
       <li @click="clickCity">
        <span>当前驾照签发城市</span>
        <span>{{info.city.join(' ')}}</span>
      </li>
    </section>
    <section>
      <van-popup v-model="showCity" position="bottom" :overlay="true">
        <van-picker :columns="cityArray" @change="cityChange" ref="cityPicker" @cancel="onCancel" show-toolbar title="请选择签发城市" @confirm="cityConfirm"/>
      </van-popup>
    </section>
     <Qiangfa />
      <p class="p">
        <span>服务费</span>
        <span class="resR">￥399</span>       
      </p>
       <p class="p1">
        <span>优惠</span>
        <span>登陆后查看优惠</span>       
      </p>
      <a href="#">常见问题？</a>
      <footer>
        <p>
          <span>实付</span>
          <span class="resR">￥399</span>
        </p>
        <p>
          <button>去支付</button>
        </p>
      </footer>
  </div>

</template>

<script>
import Upload from '@/components/Upload'
import TypePicker from '@/components/TypePicker'
import Qiangfa from '@/components/Qiangfa'
import {cityList, costList} from '@/api/index'
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
  components: {
    Upload,
    TypePicker,
    Qiangfa
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
<style>
header{
        width: 100%;
        height: 0.8rem;
        border-bottom: 1px solid #ccc;
        display: flex;
        justify-content: space-around;
        align-items: center;
        color: #33aaaa;
    }
     section li {
        width: 100%;
        height: 1.2rem;
        border-bottom:1px solid #Ccc;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0 0.1rem;
        box-sizing: border-box;
    }
 .resR{
        color:red;
        font-weight: 600;
    }
    .van-swipe img{
        width: 100%;
        height: 100%;
    }
    .p,.p1{
      width: 100%;
      height: 1.2rem;
      border-bottom:1px solid #CCC;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 0.1rem;
      box-sizing: border-box;
    }
    .p1{
      border-top:0.2rem solid #ccc;
    }
    a{
      width: 100%;
      height: 0.5rem;
      line-height: 0.5rem;
      display: inline-block;
      color:#33aaaa;
      text-align: center;
    }
    footer{
      width:100%;
      height: 0.8rem;
      display: flex;
      justify-content:space-between;
      align-items: center;
      position: fixed;
      left: 0;
      bottom: 0;
      border-top:1px solid #ccc; 
      z-index: -9;
    }
    footer p button{
      height: 0.8rem;
      width: 3rem;
      background: #Ccc;
    }
</style>