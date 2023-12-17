<template>
    
  <div id="app">
    <ContainerElement>
      <ChatWindow @messagewassent="sendMessage" v-bind:messages="messages">
        <ChatMessage v-for="message in messages" v-bind:key="message.id" v-bind:username="message.author" v-bind:datetime="message.datetime">{{ message.text }}</ChatMessage>
      </ChatWindow>
    </ContainerElement>
  </div>
  
  
</template>

<script>
import ContainerElement from './components/Container.vue'
import ChatMessage from './components/ChatMessage.vue'
import ChatWindow from './components/ChatWindow.vue'

export default {
  name: 'App',
  components: {
    ContainerElement,
    ChatMessage,
    ChatWindow
  },
  data() {
    return {
      messages: [] // создаем массив для сообщений, которые мы будем получать из компонента ChatWindow
    }
  },
  methods: {
    getUserData() {
      this.axios.get("https://657483b1b2fbb8f6509c4953.mockapi.io/chat/messages").then((response) => {
        this.messages = response.data; // Обновляем массив сообщений
      })
        .catch((error) => {
          console.log(error)
        })
        .then(() => { });
    },
    sendMessage(message) { // метод sendMessage получает объект message, из которого мы будем подставлять значения для нашего сообщения в чате
      this.axios({
        method: 'post',
        url: 'https://657483b1b2fbb8f6509c4953.mockapi.io/chat/messages',
        data: {
          author: message.author,
          text: message.text,
        }
      }).then((response) => { // выполняется в случае успешной отправки
        console.log(response);
        this.getUserData(); // После отправки сообщения обновляем сообщения снова
      })
    }
  },
  mounted() { // хук, чтобы запустить функцию в начале работы программы
    this.getUserData();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
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
