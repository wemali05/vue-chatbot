<template>
  <section class="chat-box">
    <div class="chat-bot-list-container">
        <ul class="chat-box-list">
            <li class="message"
            v-for="(message, index) in messages"
            :key="index"
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
         this.$axios.get('https://www.cleverbot.com/getreply?key=CC8uqcCcSO3VsRFvp5-uW5Nxvow&input=${message}')
         .then( res => {
             this.messages.push({
                 text: res.data.output,
                 author: 'server'
             })
         })
     }
 },
}
</script>

<style scoped>

</style>
