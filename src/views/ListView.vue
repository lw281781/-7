<script setup>
import { ref, onMounted } from 'vue'
// 渲染列表
let list = ref([{ background: 'rgb(233,32,38)' }])
//防止多次触发布尔值
let boolean = ref(true)
//loading
let boolean1 = ref(true)
//触发添加列表时距离底部距离
let scroll = 0
//添加列表
function getlist(num) {
  for (let i = 0; i < num; i++) {
    const element = {
      background: `rgb(${Math.ceil(Math.random() * 255)},
                       ${Math.ceil(Math.random() * 255)},
                       ${Math.ceil(Math.random() * 255)})`,
    }
    list.value.push(element)
  }
  if (list.value.length > 50) {
    return (boolean1.value = false)
  }
  boolean.value = true
  boolean1.value = false
}

getlist(10)

onMounted(() => {
  scroll = document.body.scrollHeight / 2
})
//模拟后台获取数据
function addlist(value) {
  if (boolean.value) {
    boolean.value = false
    boolean1.value = true
    setTimeout(() => {
      getlist(value)
    }, 3000)
  }
}

//监听页面滚动事件一但滚动超过一半时调用addlist()
window.onscroll = () => {
  let scrollTop = document.documentElement.scrollTop || document.body.scrollTop
  let scrollBody = document.body.scrollHeight
  console.log(scrollBody - scrollTop)
  if (scrollBody - scrollTop <= scroll) {
    addlist(10)
  }
}
</script>

<template>
  <div
    class="list"
    v-for="(item, index) in list"
    :key="index"
    :style="'background :' + item.background"
  >
    {{ index }}
  </div>
  <div class="loading" v-show="boolean1"></div>
</template>

<style lang="scss" scoped>
.list {
  width: 100%;
  height: 500px;
  font-size: 50px;
}
.loading {
  width: 30px;
  height: 30px;
  border: 2px solid #000;
  border-top-color: transparent;
  border-radius: 100%;
  animation: circle infinite 0.75s linear;
  margin: auto;
}
@keyframes circle {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
