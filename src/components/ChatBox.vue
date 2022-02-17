<template>
  <section class="ChatSection">
    <div class="ChatWindow" id="scrollToBottom">
      <div class="TopFade" v-if="OverFlowIsActive"></div>
      <ChatMessage v-for="chatMessage in chatMessages"
        v-bind:key="chatMessage"
        v-bind:isSelf="chatMessage.isSelf"
        v-bind:author="chatMessage.author"
        v-bind:message="chatMessage.message"/>
    </div>
    <div class="MessageOrchestrator">
      <input class="InputMessageField" v-model="message" placeholder="message" v-on:keydown.enter="send" type="text" />
      <input class="SendButton" type="button" value="Send" v-on:click="send"/>
    </div>
  </section>
</template>

<script>
import ChatMessage from './ChatMessage'
export default {
  name: 'ChatBox',
  components: {ChatMessage},
  data () {
    return {
      chatMessages: [],
      message: ''
    }
  },
  props: [],
  computed: {
    OverFlowIsActive () {
      if(!document.getElementById('scrollToBottom')) return false
      const element = document.getElementById('scrollToBottom')
      const isOverflowOn = element.offsetHeight < element.scrollHeight
      return isOverflowOn
    }
  },
  methods: {
    send: function () {
      this.addChatMessage('Me', this.message, true)
      this.sendMessage(this.message)
      this.message = undefined
    },
    sendMessage: function (message) {
      const https = require('https')
      const options = {
        host: 'larrymessenger.azurewebsites.net',
        port: 443,
        path: '/api/larrymessenger',
        method: 'POST'
      }
      const data = JSON.stringify({author: 'Berry', message: this.message})

      const req = https.request(options, res => {
        res.on('data', d => {
          console.log(d)
          this.addChatMessage('Larry', d.toString())
        })
      })

      req.on('error', error => {
        console.error(error)
      })

      req.write(data)
      req.end()
    },
    addChatMessage: function (author, message, isSelf = false) {
      this.chatMessages = this.chatMessages.concat({author: author, message: message, isSelf: isSelf})
      this.scrollToBottom()
    },
    async scrollToBottom () {
      function sleep (milliseconds) {
        return new Promise(resolve => setTimeout(resolve, milliseconds))
      }

      async function fun () {
        for (let i = 1; i <= 10; i++) {
          await sleep(25)
        }
      }
      await fun()
      document.getElementById('scrollToBottom').scrollTop = document.getElementById('scrollToBottom').scrollHeight
    }
  }
}
</script>

<style scoped>
.ChatSection{
  max-width: 500px;
  margin: auto;
}
.ChatWindow{
  margin: auto;
  border: solid 1px #aaa;
  height: 250px;
  width: auto;
  overflow-y: scroll;
  overscroll-behavior-y: contain;
  scroll-snap-type: y proximity;
  scroll-snap-align: end;
}
.TopFade{
  display: flow;
  position: absolute;
  width:100%;
  max-width: 486px;
  height:35px;
  background: linear-gradient(180deg, rgba(255,255,255,1) 0%, rgba(255,255,255,0) 100%);
}
.MessageOrchestrator{
  display: flex;
  width:auto;
  height: 30px;
}
.InputMessageField{
  display: inline-flex;
  width:100%;
  text-align: left;
  border-radius: 0 0 0 10px;
  border-width: 0px 1px 2px 2px;
  margin: 0;
  padding: 0 0 0 5px;
  height: 30px;
  border-color: #aaaaaa;
}
.SendButton{
  margin: 0;
  display: inline-flex;
  height: 32px;
  width: 80px;
  background-color: cornsilk;
  border-color: #aaaaaa;
  border-width: 0px 2px 2px 1px;
  border-radius: 0px 0px 10px 0px;
}
</style>
