<template>
<div>
   <van-swipe :autoplay="3000" indicator-color="white">
          <van-swipe-item>
              <img src="@/assets/1.png" alt="">
          </van-swipe-item>
          <van-swipe-item>
              <img src="@/assets/1.png" alt="">
          </van-swipe-item>
          <van-swipe-item>
              <img src="@/assets/1.png" alt="">
          </van-swipe-item>
          <van-swipe-item>
              <img src="@/assets/1.png" alt="">
          </van-swipe-item>
    </van-swipe>
   <ul>
    <li v-for="(item, index) in list" :key="index" @click="click(index)">
      <img v-if="item.src" :src="item.src">
      <img v-else class="add" :src="addImg">
      <p>{{item.desc}}</p>
    </li>
    <div v-show="showMask" class="mask">
      <img :src="current.demo">
      <div>
        <button @click="upload(1)">拍照</button>
        <button @click="upload(2)">相册</button>
        <button @click="cancel">取消</button>
      </div>
    </div>
  </ul>
</div>
 
</template>

<script>
import {mapState, mapMutations} from 'vuex';
import {uploadImg} from '@/api/index';
import add from '@/assets/add.png';
export default {
  data(){
    return {
      current: {},
      showMask: false
    }
  },
  name: 'Upload',
  computed: {
    ...mapState({
      list: state=>state.upload.list
    }),
    addImg(){
      return add;
    }
  },
  methods: {
    ...mapMutations({
      updateList: 'upload/updateList'
    }),
    click(index){
      this.current = this.list[index];
      this.showMask = true;
    },
    cancel(){
      this.showMask = false;
    },
    async upload(type){
      let res = await uploadImg(type);
      if (res.result == 1){
        this.updateList({
          src: res.data.url,
          index: this.list.findIndex(item=>item==this.current)
        })
        this.showMask = false;
      }else{
        alert('上传图片失败');
      }

      console.log('res...', res);
    }
  }
}
</script>

<style lang="scss" scoped>
ul{
  display: flex;
  align-items: center;
  justify-content: space-around;
}
li{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  img{
    border-radius: .15rem;
    width: .9rem;
    height: .7rem;
    border: 1px solid #c0c0c0;
  }
  p{
    font-size: .26rem;
    padding: .1rem .3rem;
    text-align: center;
  }
}
.add{
  width: .5rem;
  height: .5rem;
  padding: .1rem .2rem;
}
.mask{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0, .5);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  img{
    width: 90%;
    margin-top: 20%;
  }
  div{
    display: flex;
    width: 90%;
    flex-direction: column;
    align-items: center;
    button{
      width: 100%;
      height: .9rem;
      font-size: .4rem;
      letter-spacing: .1rem;
      border-radius: .15rem;
      color: #3aafc0;
    }
    button:first-child{
      border-radius: .15rem .15rem 0 0;
    }
    button:nth-child(2){
      border-radius: 0 0 .15rem .15rem;
    }
    button:last-child{
      margin: .15rem 0;
    }
  }
}
</style>
