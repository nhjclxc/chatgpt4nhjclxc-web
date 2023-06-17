<template> 
  
  <div class="bottom-fixed">
    <input type="text" v-model="inputText" id="login-input-box" maxlength="11" />
    <button @click="loginClick" id="login-btn">登录</button>
    <p>{{ outputText }}</p>
  </div>

</template>

<script>

const serverHost = '127.0.0.1:8899';

export default {
    data() {
      return {
        inputText: '',
        outputText: '',
      };
    },
    methods: {
      loginClick() {
        
      // 输出文本框中的值到控制台
      console.log(this.inputText);

      let token = ''
      let loginState = ''

        fetch('http://'+ serverHost +'/v1/chat/login?phone=' + this.inputText, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          }
        })
        .then(response => {
          token = response.headers.get('token');
          return response;
        })
        .then(response => response.json())
        .then(data => {
            // 判断是否请求成功
            if(data.code !== '0'){
              loginState = '登录失败'
              console.log(loginState);
              throw new Error(loginState)
            }
            loginState = '登录成功'
            console.log(loginState)
            
            // 请求成功，将token放入localStorage里面，以后请求都要带上token向服务器发请求
            console.log(token);
            // localStorage.setItem('token', token);
            sessionStorage.setItem('token', token);
        })
        .catch(error => console.error(error))

      // 将文本框中的值显示在 paragraph 元素中
      this.outputText = '你好：' + this.inputText ;


      },
    },
  };


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.bottom-fixed { float: left;
  display: block;
  position: fixed; /* 将元素的定位方式设置为固定定位 */
  top: 0; /* 将元素距离浏览器窗口底部的距离设置为 0 */
  left: 0; /* 将元素距离浏览器窗口左侧的距离设置为 0 */
  right: 0; /* 将元素距离浏览器窗口右侧的距离设置为 0 */
  height: 100px; /* 设置元素的高度 */
  /* background-color: #ccc; 设置元素的背景色 */
  padding: 20px; /* 设置元素的内边距（padding） */
  box-sizing: border-box; /* 将元素的盒模型设置为 border-box */
} 
#login-input-box {
    resize: none;
    overflow: hidden;
    height: auto;
}



h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
