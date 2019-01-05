<template>
    <div>
        <li @click="clickType">
            <span>服务类型</span>
            <span>{{type}}</span>
        </li>
        <van-popup v-model="showType" position="bottom" overlay>
            <van-picker :columns="typeArray" @cancel="onCancel" show-toolbar title="请选择服务类型" @confirm="onConfirm"/>
        </van-popup>
    </div>
</template>

<script>
export default {
    name: 'TypePicker',
    data(){
        return {
            showType: false,
            typeArray: ["补驾照", "换驾照"],
            type: ''
        }
    },
    methods: {
         onCancel(e){
            this.showType = false;
        },
        onConfirm(value){
            console.log('value...', value);
            this.type = value;
            this.onCancel();
        },
        clickType(){
            this.showType = true;
        }
    }
}
</script>
<style scoped>
    div li {
    width: 100%;
    height: 1.2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 .1rem;
    box-sizing: border-box;
    border-bottom:1px solid #CCC;
    border-top:0.2rem solid #ccc;
    }
   
</style>
