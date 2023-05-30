<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HomeView from './views/HomeView.vue';
import { BaseTransition } from 'vue';
import {ref, onMounted} from 'vue'

const minut = ref(4)
const second = ref(59)
const isStart = ref(true)
const blockStart = ref(true)

const start = () => {
  setInterval(()=> {
    blockStart.value=false
    if(isStart.value){
      if(second.value==0 && minut.value!=0){
        minut.value= minut.value-1
        second.value=60
      }
      if(second.value>0){
        second.value = second.value-1      
      }
    }
  }, 100)
}

const stop = () => {
  isStart.value=!isStart.value
}

const changeStatus = () => {
  isStart.value =false
  if(minut.value == 24){
    minut.value = 4
    second.value = 59
  }else{
    minut.value = 24
    second.value = 59
  }
}
</script>


<template>
  <div class="wrapper w-full min-h-screen bg-[#F2FFF5]">
    <div class="conatiner grid place-items-center h-screen mx-auto">
      <div class="timer">
        <div class="status"><img src="./assets/icons/ph_coffee.svg" alt=""> STATUS</div>
        <div class="numbers">
          <h1 class="flex"><h1 v-if="minut<10">0</h1>{{minut}}</h1>
          <h1 class="flex"><h1 v-if="second<10">0</h1>{{second}}</h1>
        </div>
        <div class=" flex controls gap-3 text-4xl">
          <button class="p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px]"><i class='bx bx-dots-horizontal-rounded'></i></button> 
          <button @click="blockStart? start() : stop()"  class="p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px]"> <i class='bx bx-play'></i></button> 
          <button @click="changeStatus()" class="p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px]"><i class='bx bx-skip-next' ></i></button>
        </div>
      </div>
    </div>

  </div>

</template>

<style scoped>
.numbers h1{
  font-size: 256px;
  line-height: 217px;
  font-weight: 800;
  color: #14401D;
 
}

.controls button:hover{
  background-color: rgba(96, 163, 64, 0.62);
}

.controls button{
  transition: all 0.3s ease;
}
.controls button:focus{
  padding: 32px 48px;
  background-color: rgba(77, 218, 110, 0.62);
}

.controls{
  height: 100px;
  display: flex;
  align-items: center;
}


.numbers{
  margin: 32px 0;
}

.status{
  width: 197px;
  height: 48px;
  border: 2px solid #14401D;
  border-radius: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  background-color: rgba(77, 218, 110, 0.15);;
}

.timer{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}


</style>
