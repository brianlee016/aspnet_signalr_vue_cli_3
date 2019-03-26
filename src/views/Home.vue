<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div>
      <p>Hello</p>
      <ul>
        <li v-for="(message, index) in messages">
          {{message}}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
const $ = require('jquery')
window.jQuery = $;
require('signalr')


export default {
  name: 'home',
  data(){
    return {
      connection: null,
      hub: null,
      messages: []
    }
  },
  components: {
    
  },
  created(){
    let connection = $.hubConnection("http://localhost:61053/signalr", { useDefaultPath: false });
    let hubProxy = connection.createHubProxy('testHub');
      hubProxy.on("helloWorld", (message) => {
        console.log("hello world received!", message);
        this.messages.push(message);
      });

    connection.start()
    .done(() => { 

      console.log('Now connected, connection ID=' + connection.id); 
      })
    .fail(() => { console.log('Could not connect'); });

    

    
  }
}
</script>
