<template>
  <section class="chat-box">
    <div class="chat-box-list-container" ref="chatbox">
        <ul class="chat-box-list">
            <li class="message"
            v-for="(message, index) in messages"
            :key="index"
            :class="message.author"
            >
             <p><span>{{ message.text }}</span></p>   
            </li>
        </ul>
    </div>
    <div class="chat-inputs">
        <input type="text"
         v-model="message" 
         @keyup.enter="sendMessage" 
         />
        <button @click="sendMessage">Send</button>
    </div>
  </section>
</template>

<script>
export default {
  name: 'ChatBox',
 data() {
     return {
         message: '',
         messages: []
     }
 },
 methods: {
     sendMessage(){
         this.messages.push({
             text: this.message,
             author: 'client'
         })
         this.message = ''   

         this.$axios.get('https://www.cleverbot.com/getreply?key=CC8uqcCcSO3VsRFvp5-uW5Nxvow&input=${message}')
         .then( res => {
             this.messages.push({
                 text: res.data.output,
                 author: 'server'
             })
               this.$nextTick(() => {
               this.$refs.chatbox.scrollTop = this.$refs.chatbox.scrollHeight
           })
         })
     }
 }
}
</script>

<style scoped lang="scss">
.chat-box,
.chat-box-list {
    display: flex;
    flex-direction: column;
    list-style-type: none;
}
.chat-box{
    border: 1px solid #999;
    width: 50vw;
    height: 50vh;
    border-radius: 4px;
    margin-left: auto;
    margin-right: auto;
    align-items: space-between;
    justify-content: space-between;
}

.chat-box-list-container{
    overflow: scroll;
}

.chat-box-list{
    margin: 10px;
    padding-left: 0;

    span {
        padding: 8px;
        color: white;
        border-radius: 4px;
    }

    .server {
       span{
            background: green
       }
       p{
           float: left
       }
    }
    .client {
       span {
            background: blue;
       }
       p{
           float: right;
       }
    }
}

.chat-inputs{
    display: flex;

    input{
        line-height: 3;
        width: 100%;
        border: 1px solid #999;
        border-left: none;
        border-bottom: none;
        border-right: none;
        border-bottom-left-radius: 4px;
        padding-left: 15px;
    }

    button{
        width: 145px;
        color: white;
        background-color: green;
        border-bottom-right-radius: 4px;
    }
}

</style>
