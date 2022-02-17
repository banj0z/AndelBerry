<template>
<div :class="ChatMessageClass">
  <div :class="MessageAuthorClass">{{author}}:</div><div class="MessageContent">{{message}}</div>
</div>
</template>

<script>
export default {
  name: 'ChatMessage',
  props: {
    author: String,
    message: String,
    isSelf: Boolean
  },
  computed: {
    ChatMessageClass () {
      return this.isSelf ? 'ChatMessage__Self' : 'ChatMessage'
    },
    MessageAuthorClass () {
      return this.isSelf ? 'MessageAuthor__Self' : 'MessageAuthor'
    }
  }
}
</script>

<style scoped>
.ChatMessage{
  display:flex;
  background-color: #dfd;
}
.ChatMessage__Self{
  display:flex;
}

.MessageAuthor{
  display: inline-flex;
  -ms-flex-align: start;
  font-weight: bold;
  color: #42b983;
  width:50px;
  padding: 5px;
}

.MessageAuthor__Self{
  display: inline-flex;
  -ms-flex-align: start;
  font-weight: bold;
  color: #888;
  width:50px;
  padding: 5px;
}

.MessageContent{
  display: inline-flex;
  overflow-wrap: anywhere;
  padding-left: 5px;
}
</style>
