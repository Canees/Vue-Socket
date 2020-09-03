<template>
  <div class="home">
    <div class="chat">
      <div class="chat-container">
        <ul class="chatList">
          <li v-for="(item,index) in msgList" :key="index">
            <div v-if="item.name == 'U.amazing'" class="me">
              <div class="content">
                <p>姓名：{{item.name}}</p>
                <p>消息:{{item.msg}}

                </p>
              </div>
              <div class="img">
                <img src="./assets/chat/user.png" alt="">
              </div>
            </div>
            <div v-else class="other">
              <div class="img">
                <img src="./assets/chat/robot.png" alt="">
              </div>
              <div class="content">
                <p>姓名：{{item.name}}</p>
                <p>消息:{{item.msg}}</p>
                <p>
                  <img style="width:100px;height:100px" :src="item.msg" alt="">
                </p>
              </div>
            </div>
          </li>
        </ul>
      </div>

      <div class="chat-input">
        <input type="textarea" :autosize="{ minRows: 4, maxRows: 6 }" maxlength="300" placeholder="请输入内容" v-model="msg" />
        <button @click="send()">发送</button>
      </div>
    </div>
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
  created () {
    console.log(this.$socket)
  },
  sockets: {
    connect: function () {
      const id = this.$socket.client.id
      console.log('建立连接', id + ' connected')
    },
    customEmit: function (data) {
      console.log(data)
      console.log('this method was fired by the socket server. eg: io.emit("customEmit", data)')
    },
    res: function (data) { //接受到消息后调用的函数
      this.msgList.push(data)
      console.log('接收到服务端消息', data);
    },
    close: function (params) {
      console.log('取消socekt', params)
    }
  },
  methods: {
    send () {
      const db = {
        "collection": "heyanlu_left",
        "startRing": 937,
        "endRing": 938,
        "ring": "property.S9539",
        "data": {
          "timestamp": 1,
          "year": 1,
          "month": 1,
          "day": 1,
          "hour": 1,
          "minute": 1,
          "property.S9539": 1,
          "property.SMODE": 1,
          "property.S1140": 1,
          "property.S1139": 1,
          "property.S1142": 1
        }
      }
      // 给服务器发送一个字符串:
      this.msgList.push({
        name: 'U.amazing',
        msg: db
      })
      this.$socket.client.emit('server', db)
      this.msg = ''
    }
  }
}
</script>

<style lang="stylus" scope>
.home {
  .chat {
    &-container {
      padding: 10px;
      border: 1px solid rgba(0, 0, 0, 0.1);

      .chatList {
        li {
          padding: 20px 0;
        }

        .img {
          width: 50px;
          height: 50px;
          border-radius: 50%;
          overflow: hidden;
          box-shadow: 0 0 3px rgba(0, 0, 0, 0.4);

          img {
            width: 100%;
            height: 100%;
          }
        }

        .me {
          display: flex;
          justify-content: flex-end;

          .content {
            margin-right: 10px;
            padding: 5px;
            border: 1px solid #ccc;
            border-radius: 5px;
          }
        }

        .other {
          display: flex;
          justify-content: flex-start;

          .content {
            margin-left: 10px;
            padding: 5px;
            border: 1px solid #ccc;
            border-radius: 5px;
          }
        }
      }
    }

    &-input {
      padding: 10px;
      border: 1px solid rgba(0, 0, 0, 0.1);
      border-top: none;
    }
  }
}
</style>