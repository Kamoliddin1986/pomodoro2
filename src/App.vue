<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HomeView from './views/HomeView.vue';
import { BaseTransition } from 'vue';
import {ref, onMounted, reactive} from 'vue'
import WorkTimeEnd from './assets/WorkTimeEnd.mp3'
import RestTimeEnd from './assets/RestTimeEnd.mp3'

const minut = ref(25)
const second = ref(0)
const isStart = ref(false)
const setTime = ref()
const status = ref('work_time')
const workTime = ref(25)
const restTime = ref(5)
const outerVisible = ref(false)
const isResume = ref(false)
const isSound = ref(false)


const colors = reactive({
    bg: '#FFF2F2',
    main: '#471515',
    btn_hover: '#ff4c4c',
    bg_btn: '#FF4C4CB5'
})


const changeColor = () => {
  if(status.value == 'work_time'){
    colors.bg = '#F2FFF5'
    colors.main = '#14401D'
    colors.btn_hover = '#4DDA6E9E'
    colors.bg_btn = '#4DDA6E26'
  }else{
    colors.bg= '#FFF2F2'
    colors.main= '#471515'
    colors.btn_hover= '#ff4c4c'
    colors.bg_btn= '#FF4C4CB5'
  }
}

const startStop = () => {
  isStart.value=!isStart.value
  console.log(isStart.value);
  if(isStart.value){
  setTime.value = setInterval(()=> {
      if(second.value==0 && minut.value!=0){
        minut.value= minut.value-1
        second.value=60
      }else if(second.value==0 && minut.value==0){
        changeStatus(!isResume.value)
      }
      if(second.value>0){
        second.value = second.value-1      
      }
    }, 10)
  }else{
    clearInterval(setTime.value)
  }
}



const changeStatus = (rez=1) => {
  if(rez){
    isStart.value = false
    clearInterval(setTime.value)
  }
  changeColor()
  if(isSound.value){
    play()
  }
  second.value = 0
  if(status.value == "work_time" ){
          status.value = "rest_time"
          minut.value = restTime.value
        }else{
          status.value = "work_time"
          minut.value = workTime.value
        }
}



const play = () => {
  const WorkTimeEndSound = document.querySelector('#song1')
  const RestTimeEndSound = document.querySelector('#song2')
  if(status.value=='work_time'){
    WorkTimeEndSound.play()
  }else{
    RestTimeEndSound.play()
  }

}
</script>


<template>
<!-- New Modal -->
<el-dialog v-model="outerVisible" title="Outer Dialog">
  <template #default>  
    <div class="flex items-center text-lg font-bold">
      Settings
    </div>
  </template>
  <template #footer>
    
    <div class="flex flex-col items-between justify-between gap-2 ">


      <div class="flex items-center justify-between">
        <p>Focus length</p>
        <el-input-number v-model="workTime" :min="0" :max="60" size="small" controls-position="right" @change="handleChange" class="number_input" />
      </div>

      <div class="flex items-center justify-between">
        <p>Short break length</p>
        <el-input-number v-model="restTime" :min="0" :max="60" size="small" controls-position="right" @change="handleChange" class="number_input" />
      </div>


      <div class="flex items-center justify-between">
        <p>Auto resume timer</p>
        <el-switch v-model="isResume" />
      </div>
      <div class="flex items-center justify-between">
        <p>Sound</p>
        <el-switch v-model="isSound" />
      </div>

    </div>
    
  </template>
</el-dialog>
<!-- endModal -->

  <div class="wrapper w-full min-h-screen">
    <audio :src="WorkTimeEnd" controls id="song1" class="h-0 w-0"></audio>
    <audio controls :src="RestTimeEnd" id="song2" class="h-0 w-0"></audio>
    <div class="conatiner grid place-items-center h-screen mx-auto">
      <div class="timer">
        <div v-if="status=='work_time icon'" class="status file:icon">
          <img  src="./assets/icons/ph_brain-fill.svg" alt="" >FOCUS
        </div>
        <div v-else class="status icon">
          <img  src="./assets/icons/ph_coffee.svg" alt="" >Short Break
        </div>
        <div class="numbers">
          <h1 class="flex"><h1 v-if="minut<10">0</h1>{{minut}}</h1>
          <h1 class="flex"><h1 v-if="second<10">0</h1>{{second}}</h1>
        </div>
        <div class=" flex controls gap-3 text-4xl">
          <button @click="outerVisible = true" class="butn1 p-6  rounded-[24px]"><i class='bx bx-dots-horizontal-rounded'></i></button> 
          <button @click="startStop()"  class="butn1 p-6 rounded-[24px]"> <i v-if="!isStart" class='bx bx-play'></i> <i v-else class='bx bx-pause'></i></button> 
          <button @click="changeStatus()" class="butn1 p-6 rounded-[24px]"><i class='bx bx-skip-next' ></i></button>
        </div>
      </div>
    </div>

  </div>

</template>

<style scoped>

.wrapper{
  background-color: v-bind('colors.bg');
}
.numbers h1{
  font-size: 256px;
  line-height: 217px;
  font-weight: 800;
  color: v-bind('colors.main');
 
}

.butn1:hover{
  background-color: v-bind('colors.btn_hover');
}
.butn1{
  transition: all 0.3s ease;
  background-color: v-bind('colors.bg_btn');
}


.butn1:focus{
  padding: 32px 48px;
  background-color: v-bind('colors.btn_hover');
}



.controls{
  height: 100px;
  display: flex;
  align-items: center;
}


.numbers{
  margin: 32px 0;
  color: v-bind('colors.main')
}
.icon{
  color: v-bind('colors.main')
}

.status{
  width: 197px;
  height: 48px;
  border: 2px solid v-bind('colors.main');
  border-radius: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  background-color: v-bind('colors.bg_btn');
}

.timer{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}


</style>
