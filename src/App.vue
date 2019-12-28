<template>
  <div id="app">
    <Container>
      <ChatWindow @send-message="sendMessage">
        <ChatMessage  v-for="smth in text" :key="smth.id" :username="smth.author" :datetime="smth.datetime">{{ smth.text }}</ChatMessage>
      </ChatWindow>
    </Container>
  </div>
</template>

<script>
import Container from './components/Container'
import ChatMessage from './components/ChatMessage'
import ChatWindow from './components/ChatWindow'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    Container,
    ChatMessage,
    ChatWindow
  },
  data() {
    return {
      text: []
    }
  },
  methods: {
    getMessages(){
      setInterval(() => {
        axios.get('http://188.225.47.187/api/chat/getmessages.php').then((response) => {
        this.text = response.data
        console.log('responce', response.data)
        })
      }, 1000)
    },
    sendMessage(obj){
      obj.nickname, obj.message
      axios.post('http://188.225.47.187/api/chat/sendmessage.php', obj)({
        author: obj.nickname,
        text: obj.message,
      }).then((responce) =>{
        console.log("obj", obj)
        console.log('rrr', responce)
        this.getMessages()
      })
    },
  },
  mounted() {
    this.getMessages()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  margin: 0;
  background-color: #f9f9fa;
}
</style>
