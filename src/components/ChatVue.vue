<template> 


    <div class="bottom-fixed">
    <!-- <input type="text" v-model="inputText" id="input-box" /> -->
    <textarea v-model="inputText" id="input-box"  rows="3"  cols="30" maxlength="100" />
    <button @click="handleClick" id="submit-btn">发送</button>
  </div>


  <!-- <ul id="myList"></ul> -->

  <div class="scrollable"> 
    <ul id="myList"></ul>
  </div>


</template>

<script>

const serverHost = '127.0.0.1:8899';



window.onload = function(){

  // 获取输入框和提交按钮的 DOM 对象
  var inputBox = document.getElementById('input-box');
  var submitBtn = document.getElementById('submit-btn');

  // 注册回车键按下事件的监听器
  inputBox.addEventListener('keypress', function(event) {
    if (event.key === 'Enter') { // 如果按下的是回车键
      event.preventDefault(); // 阻止默认行为（防止页面刷新）
      submitBtn.click(); // 触发按钮的点击事件
    }
  });

}

var chatJsonData
var recordSn

var contentArray = []
// console.log('contentArray = ' + contentArray[contentArray.length - 1])


export default {
    data() {
      return {
        inputText: '',
        outputText: '',
      };
    },
    methods: {
      handleClick() {
        // 输出文本框中的值到控制台
        console.log(this.inputText);

        // 将文本框中的值显示在 paragraph 元素中
        // this.outputText = this.inputText;

      chatJsonData = {
        "content": this.inputText,
        "recordSn": recordSn
      };
      console.log(chatJsonData) 

      addLi('ME：' + this.inputText, '#FFFFFF');
      this.inputText = '';// 清空文本框


      // 判断是否登录
      let token = localStorage.getItem('token')
      console.log(token  + '  token')
      if (token === null){
        // throw new Error('请先登录')
          addLi('System-web：请先登录', '#FFC0CB');
          return ;
      }

      // 发请求
      fetch('http://'+ serverHost +'/v1/chat/chat', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'token': token
        },
        body: JSON.stringify(chatJsonData)
      })
      .then(response => response.json())
      .then(data => {
        console.log(data)
        
        if('0' === data.code){ 
          recordSn = data.data.recordSn
          console.log(recordSn)
          // 将文本框中的值显示在 paragraph 元素中
          // this.outputText = data.data.content;
          contentArray[contentArray.length] =  data.data.content;
          
          
          console.log('contentArray22 = ' + contentArray[contentArray.length - 1])
          // contentArray.forEach(function(element, index) {
            // console.log(`Element ${element} is at index ${index}`);
          // });
            
          addLi('GPT：' + data.data.content, '#F5F5F5');
        }else{
          console.log('请求失败')
          addLi('System：' + data.msg, '#FFC0CB');
        }

        return data;
      })
      .catch(error => console.error(error))

      },
    },
  };

  function addLi(content, color) {
      const myList = document.getElementById("myList"); // 获取 ul 元素
      const li = document.createElement("li"); // 创建 li 元素
      li.style.backgroundColor = color; //添加背景色
      const text = document.createTextNode(content); // 创建文本节点，并将数组中的元素作为内容
      li.appendChild(text); // 将文本节点添加到 li 元素中
      myList.appendChild(li); // 将 li 元素添加到 ul 元素中

      const li2 = document.createElement("li"); // 创建 li 元素
      li.style.backgroundColor = color; //添加背景色
      const text2 = document.createTextNode(''); // 创建文本节点，并将数组中的元素作为内容
      li2.appendChild(text2); // 将文本节点添加到 li 元素中
      myList.appendChild(li2); // 将 li 元素添加到 ul 元素中
  }


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

li {
/* 取出li标签前面的冒号 */
  list-style-type: none;
  /* 内容居左显示 */
  text-align: left;
} 

.scrollable {
  height: 500px; /* 设置元素的高度 */
  overflow: auto; /* 让内容超出元素范围时出现滚动条 */
  overflow-y: hidautoden; /* 允许垂直方向上的滚动条 */
  overflow-x: hidden; /* 禁止水平方向上的滚动条 */
}
.bottom-fixed { float: left;
  display: block;
  position: fixed; /* 将元素的定位方式设置为固定定位 */
  bottom: 0; /* 将元素距离浏览器窗口底部的距离设置为 0 */
  left: 0; /* 将元素距离浏览器窗口左侧的距离设置为 0 */
  right: 0; /* 将元素距离浏览器窗口右侧的距离设置为 0 */
  height: 100px; /* 设置元素的高度 */
  /* background-color: #ccc; 设置元素的背景色 */
  padding: 20px; /* 设置元素的内边距（padding） */
  box-sizing: border-box; /* 将元素的盒模型设置为 border-box */
}
#input-box {
  /* width: 200px; */
  /* height: 20px; */
    resize: none;
    overflow: hidden;
    height: auto;
    min-height: 60px;
    font-size: 1.2rem;
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
