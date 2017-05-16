<template>
  <div id="app">

    <!-- <router-view></router-view> -->

    <ul>
      <li v-for="message in messages" class="chat-message">
        <strong>
          {{message.name}}
        </strong>
        <p>
          {{message.body}}
        </p>
      </li>
    </ul>
    <form @submit="postMessage">
      <input type="text" placeholder="Nombre" v-model="name">
      <textarea rows="8" cols="80" placeholder="Mensaje completo" v-model="newMessage"></textarea>
      <button type="submit" >Publicar</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import feathers from 'feathers/client'
import socketio from 'feathers-socketio/client'
import io from 'socket.io-client'

export default {
  name: 'app',
  data(){
    return {
      messages: [] ,
      name: '',
      newMessage: ''
    }
  },
  mounted: function (){
    self = this

    const app = feathers().configure( socketio( io('http://localhost:3030') ) )
    app.service('messages').on('created', function (message){
      self.messages.push(message);
    })

      return axios.get('http://localhost:3030/messages')
      .then(
        function (response){
          response.data.data.map(function(elemento){
            self.messages.push( elemento )
          })
          self.messages.name('')
          self.messages.newMessage('')
        })
      .catch(function (error)
      {
          console.log(error);
      })

  },
  methods:{
    postMessage:function(e){
      self = this
      e.preventDefault();
      axios.post('http://localhost:3030/messages',{
        body: this.newMessage,
        name: this.name
      })
      .then(function(){
        self.newMessage = ''
      })
    }
  }
}
</script>

<style>
body{
  margin: 50px;
  font-family: sans-serif;
}
.chat-message{
  list-style: none;
  margin-bottom: 20px;
  border-bottom: 1px solid #e5e5e5;
}
ul{
  margin: 0;
  padding: 0;
}
form input{
  display: block;
  border-bottom: 1px solid #e5e5e5;
  width: 100%;
  padding: 10px;
  margin: 20px 0;
  border: 0px none;
}
form textarea{
  border: 0px none;
  border-bottom: 1px solid #e5e5e5;
  padding: 10px;
  display: block;
  width: 100%;
}
</style>
