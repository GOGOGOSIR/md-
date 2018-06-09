# websocket心跳

```
// 创建一个websocket对象
createWebSocket() {
  // 创建一个socket
  let token = localStorage.getItem('token')
  if (!token) {return}
  this.socketUrl = `ws://${socketUrl}?token=${token}`
  try {
    this.ws = new WebSocket(this.socketUrl);
    // 初始化socket的方法
    this.webSocketInit()
  } catch(e) {
    // 创建失败进行重连
    this.reconnect()
  }
},
// 从客户端发送数据给服务器端
sendData(ws) {
	ws.send('1')
},



webSocketInit() {
  // 初始化socket的方法
  let _this = this
  this.ws.onopen = function(evt) {
    if ( _this.ws.readyState == 1) {
      if (_this.timeoutObj) {
      clearInterval(_this.timeoutObj)
      }
      _this.sendData(_this.ws)
      _this.timeoutObj = setInterval(() => {
     	 _this.sendData(_this.ws)
      }, 90000)
    }
  }
  this.ws.onmessage = function(evt) {
    let obj = JSON.parse(evt.data)
    if (obj.code == 40001) {
      if (_this.timeoutObj) {
      	clearInterval(_this.timeoutObj)
      }
      _this.is_error_token = true
      _this.ws.close()
      _this.$message.error('登录状态已过期，请重新登录')
    } else{
      // 重新请求数据
      _this.is_error_token = false
      _this._getNoReadMsg()
      if (_this.is_show_alarm) {
        let arr = ['_getNoReadAlarmMsgInfo', '_getNoReadPlanMsgInfo', '_getNoReadBaseMsgInfo']
        _this[arr[obj.msg_type - 1]]()
        _this.$refs.alarm.setIndex(obj.msg_type - 1)
      }     
    }
  }
  this.ws.onclose = function(evt) {
     // 重新连接 当没有丢失token的时候重连
     if (!_this.is_error_token) {
       _this.reconnect()
     } else {
       _this.$router.push({path: '/login'})
     }
  } 
  this.ws.onerror = function (event) {
      // 重新连接
      if (!_this.is_error_token) {
         _this.reconnect()
      }
  }
},


// 重连
reconnect() {
  if (this.lockReconnect) {
  	// 代表已经在连接了
  	return
  }
  this.lockReconnect = true
  if (this.reconnectTimer) {
  	clearTimeout(this.reconnectTimer)
  }
  this.reconnectTimer = setTimeout(() => {
  	this.createWebSocket()
  	this.lockReconnect = false
  }, 4000)
},
```