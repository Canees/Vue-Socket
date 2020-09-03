<template>
  <div>

  </div>
</template>
<script>
export default {
  data () {
    return {
      msg: '',
      msgList: [],
    }
  },
  mounted () {
    console.log(this)
  },
  sockets: {
    connect: function () {
      const id = this.$socket.client.id
      console.log(id + ' connected')
    },
    customEmit: function (data) {
      console.log(data)
      console.log('this method was fired by the socket server. eg: io.emit("customEmit", data)')
    },
    res: function (data) {
      this.msgList.push(data)
      console.log('接收到服务端消息', data);
    }
  },
  methods: {
    send () {
      // 给服务器发送一个字符串:
      console.log('send')
      this.msgList.push({
        name: 'U.amazing',
        msg: this.msg
      })
      this.$socket.client.emit('server', {
        name: 'U.amazing',
        msg: this.msg
      })
      this.msg = ''
    }
  }
}