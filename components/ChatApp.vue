<template>
<div>
   <div v-if="!joined" class="parent">
    <div class="container">
    ระบบปิดเเล้ว รออัปเดตนะงับ
        <input type="text" class="username" v-model="currentUser" />
        <button class="btn" @click="join"> Join</button>
    </div>
   </div>
   <div v-if="joined">
    <div class="container-list">
    <div v-for="message in message" :key="message.id">
        <b>
{{message.user}} 
        </b>
        : {{message.text}}
    </div>
</div>
<div class="container-text">
    <textarea class="text-area" v-model="text" @keyup.enter="sendMessage" />
</div>
   </div>
   </div>
</template>
<script lang="ts">
type Data = {
    joined:Boolean
    currentUser:String
    text:String
    message:Array<object>
    socketInstance:any
    }
import Vue from 'vue'
import io from 'socket.io-client'
export default Vue.extend({
    name:'ChatApp',
    data() : Data{
        return{
            joined:false,
            currentUser:'',
            text:'',
            message:[],
            socketInstance:null
        }
    },methods:{
join(){
    this.joined = true
    this.socketInstance = io('https://0116-124-120-22-143.ap.ngrok.io')
    this.socketInstance.on(
        'message:received',(data:any)=>{
           this.message.push(data)
        }
    )
},
sendMessage(){
this.addtext()
this.text=''
},
addtext(){
        const messages:any = {
        id:new Date().getTime(),
        user:this.currentUser,
        text:this.text,
        time:new Date().getTime()
    }
    this.message.push(messages)
    this.socketInstance.emit('message',messages)
}
    }

})
</script>
<style scoped>
.container-text{
    height: 500px;
    overflow-x: auto;
}
.text-area{
width: 100%;
position: absolute;
bottom: 0px;
height: 70px;
padding:10px;
box-sizing: border-box;
}
.parent{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    position:fixed;
    padding-top:150px;
}
.container{
    display:flex;
    flex-direction: column;
    width: 200px;

}
.btn{
    height: 30px;
font-size: 20px;
}
.username{
    height: 30px;
    font-size: 20px;
    padding: 5px;
    margin-bottom:5px;
    text-align: center;
    font-weight: bold;
}
</style>
