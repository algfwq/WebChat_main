<script setup>
import { ref, onMounted, onUnmounted,reactive } from 'vue'
import Cookies from 'js-cookie';

//设置深色模式
import { useDark, useToggle } from '@vueuse/core'

const isDark = useDark()
const toggleDark = useToggle(isDark)

//设置骨架屏
const loading = ref(true)

//骨架屏高度自适应
function autoheight1(id){
  function adjustheight(id) {
    // 获取指定div元素对象
    var divElement = document.getElementById(id);
    divElement.style.height = (window.innerHeight - 40).toString() + "px";
  }
  window.setInterval(function logname() {adjustheight(id)}, 1);
}
autoheight1("sk_aside");
function autoheight2(id){
  function adjustheight(id) {
    // 获取指定div元素对象
    var divElement = document.getElementById(id);
    divElement.style.height = (window.innerHeight - 60).toString() + "px";
  }
  window.setInterval(function logname() {adjustheight(id)}, 1);
}
autoheight2('sk_main');

//设置页面标题
let login_title = ref("WebChat")

// 获取当前页面的主机名和端口号
const protocol = window.location.protocol === 'https:' ? 'wss:' : 'ws:';
const host = window.location.host;
const url = `${protocol}//127.0.0.1:8000/main_ws/`;

//创建websocket连接后端
var socket = new WebSocket(url)
console.log("websocket连接成功！")

//定义断开websocket连接之后的回调函数
socket.onclose = function(evt) {
  console.log("Websocket连接已经关闭！");
};

socket.onmessage = function(message){
  //输出消息，方便debug
  console.log(message.data)
  //处理服务器消息为JSON
  var data = JSON.parse(message.data);
  //输出模式
  console.log(data.mode)

}

</script>

<template>
  <!-- 内容骨架屏 -->
  <el-skeleton style="width: 100%" direction="vertical" alignment="flex-start" :loading="loading" animated class="content-skeleton">
    <!-- 骨架屏内容 -->
    <template #template>
      <el-container width="100%">
        <el-aside width="50px"><el-skeleton-item variant="rect" style="width: 100%" id="sk_aside"/></el-aside>
        <el-main><el-skeleton-item variant="rect" style="width: 100%" id="sk_main"/></el-main>
      </el-container>
    </template>
    <!-- 正文 -->
    <template #default>
      <el-container width="100%">
        <el-aside width="50px"></el-aside>
        <el-main></el-main>
      </el-container>
    </template>
  </el-skeleton>
</template>

<style scoped>
.el-container {
  height: 100%;
}
</style>