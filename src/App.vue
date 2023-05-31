<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HomeView from './views/HomeView.vue';
import { BaseTransition } from 'vue';
import {ref, onMounted} from 'vue'
import ding from './assets/ding.mp3'

const minut = ref(25)
const second = ref(0)
const isStart = ref(false)
const setTime = ref()
const status = ref('work_time')
const workTime = ref(25)
const restTime = ref(5)
const long_break = ref()
const poms = ref()


const startStop = () => {
  isStart.value=!isStart.value
  console.log(isStart.value);
  if(isStart.value){
  setTime.value = setInterval(()=> {
      if(second.value==0 && minut.value!=0){
        minut.value= minut.value-1
        second.value=60
      }else if(second.value==0 && minut.value==0){

        changeStatus()
      }

      
      if(second.value>0){
        second.value = second.value-1      
      }
    }, 100)
  }else{
    clearInterval(setTime.value)
  }
}



const changeStatus = () => {
  isStart.value = false
  clearInterval(setTime.value)
  second.value = 0
  if(status.value == "work_time" ){
          status.value = "rest_time"
          minut.value = restTime.value
        }else{
          status.value = "work_time"
          minut.value = workTime.value
        }
}


const isShowModal = ref(false)

const closeModal = () => {
  isShowModal.value = false
}
const showModal = () => {
  isShowModal.value = true
}
</script>


<template>

  <Modal size="md" v-if="isShowModal" @close="closeModal">
    <template #header>
      <div class="flex items-center text-lg font-bold">
        Settings
      </div>
    </template>
    <template #body>
      
      <div class="flex flex-col items-between justify-between gap-2 ">
        <div class="flex items-center justify-between">
          <p>Dark Mode</p>
          <el-switch v-model="isDark" />
        </div>

        <div class="flex items-center justify-between">
          <p>Focus length</p>
          <el-input-number v-model="workTime" :min="0" :max="60" size="small" controls-position="right" @change="handleChange" class="number_input" />
        </div>
        <div class="flex items-center justify-between">
          <p>Pomodoros until long break</p>
          <el-input-number v-model="poms" :min="0" :max="60" size="small" controls-position="right" @change="handleChange" class="number_input" />
        </div>
        <div class="flex items-center justify-between">
          <p>Short break length</p>
          <el-input-number v-model="restTime" :min="0" :max="60" size="small" controls-position="right" @change="handleChange" class="number_input" />
        </div>
        <div class="flex items-center justify-between">
          <p>Long break length</p>
          <el-input-number v-model="long_break" :min="0" :max="60" size="small" controls-position="right" @change="handleChange" class="number_input" />
        </div>

        <div class="flex items-center justify-between">
          <p>Auto resume timer</p>
          <el-switch v-model="isResume" />
        </div>
        <div class="flex items-center justify-between">
          <p>Sound</p>
          <el-switch v-model="isSound" />
        </div>
        <div class="flex items-center justify-between">
          <p>Notifications</p>
          <el-switch v-model="isNotify" />
        </div>
      </div>
      
    </template>
    
  </Modal>
  <div class="wrapper w-full min-h-screen bg-[#F2FFF5]">
    <audio :src="ding" controls id="song" class="h-0 w-0"></audio>
    <div class="conatiner grid place-items-center h-screen mx-auto">
      <div class="timer">
        <div v-if="status=='work_time'" class="status">
          <img  src="./assets/icons/ph_brain-fill.svg" alt="">FOCUS
        </div>
        <div v-else class="status">
          <img  src="./assets/icons/ph_coffee.svg" alt="">Short Break
        </div>
        <div class="numbers">
          <h1 class="flex"><h1 v-if="minut<10">0</h1>{{minut}}</h1>
          <h1 class="flex"><h1 v-if="second<10">0</h1>{{second}}</h1>
        </div>
        <div class=" flex controls gap-3 text-4xl">
          <button @click="showModal" class="p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px]"><i class='bx bx-dots-horizontal-rounded'></i></button> 
          <button @click="startStop()"  class="p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px]"> <i class='bx bx-play'></i></button> 
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
