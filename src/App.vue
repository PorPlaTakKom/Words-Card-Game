<script setup>
import Card from "./components/Card.vue";
import {onUpdated, ref} from "vue";

let data = ref([{en: 'ant', th: 'มด'}, {en: 'apple', th: 'แอปเปิล'}, {en: 'colors', th: 'สี'}, {en: 'cat', th: 'แมว'}])
let isAddWord = ref(false)
let errorMsg = ref({engWord: false, thaiWord: false})
let isSelectMode = ref(false)
let isGameStart = ref(false)
let gameMode = ref("")
let newWord = {}
let question = ref(null)
let arrayOfAns = ref([])
let ans = ref("")
let isCheckAns = ref(false)
const toggleAddWord = () => {
  isAddWord.value = !isAddWord.value
}

const toggleGameMode = () => {
  isSelectMode.value = !isSelectMode.value
}

const toggleGameStart = () => {
  isGameStart.value = !isGameStart.value
}

const checkWords = (event) => {
  const eng_pattern = /^[a-zA-Z]+$/
  const th_pattern = /^[\u0E00-\u0E7F]+$/

  if (event.target.id === 'eng' && eng_pattern.test(event.target.value)) {
    newWord = {...newWord, en: event.target.value}
    errorMsg.value.engWord = false
  } else if (event.target.id === 'eng' && eng_pattern.test(event.target.value) === false) {
    errorMsg.value.engWord = true
  }

  if (event.target.id === 'th' && th_pattern.test(event.target.value)) {
    newWord = {...newWord, th: event.target.value}
    errorMsg.value.thaiWord = false
  } else if (event.target.id === 'th' && th_pattern.test(event.target.value) === false) {
    errorMsg.value.thaiWord = true
  }
}
const handleSubmitAddWord = () => {
  data.value.push(newWord)
  newWord.value = {}
  isAddWord.value = !isAddWord.value
}

const randomCard = () => {
  const randomNumber = Math.floor(Math.random() * data.value.length);

  if (question.value === null) {
    question.value = data.value[randomNumber]
    arrayOfAns.value = data.value.filter(item => item.en !== question.value.en).slice(0, 3)
    arrayOfAns.value.push(data.value[randomNumber])
    shuffleArray()
    return question.value
  } else {
    return question.value
  }
}

const shuffleArray = () => {
  for (let i = arrayOfAns.value.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [arrayOfAns.value[i], arrayOfAns.value[j]] = [arrayOfAns.value[j], arrayOfAns.value[i]];
  }
}

const checkAns = () =>{
  isCheckAns.value = true
  return ans.value === (gameMode.value === 'en' ? question.value.en : question.value.th)
}

const restartGame = () =>{
  question.value = null
  arrayOfAns.value = null
  isCheckAns.value = false
  ans.value = ""
  randomCard()
}

const cancelGame = () =>{
  question.value = null
  arrayOfAns.value = null
  isCheckAns.value = false
  ans.value = ""
  isAddWord.value = false
  isSelectMode.value = false
  isGameStart.value = false
  gameMode.value = ""
}
</script>

<template>
  <div class="w-screen h-screen">
    <div class="flex flex-col gap-2 w-full justify-center items-center m-auto h-full sm:bg-blue-100 font-sanam-deklen">
      <div class="flex sm:w-2/3 sm:h-1/6 mt-10 justify-center items-center m-auto">
        <h1 class="font-bold text-4xl sm:text-6xl text-red-300">Words Card Game</h1>
      </div>

      <div class="flex flex-col justify-center items-center w-11/12 sm:w-2/3 h-5/6 bg-blue-100 sm:bg-white rounded-xl m-10 p-5 overflow-hidden">
        <div v-if="(!isAddWord && data.length === 0)" class="flex w-full h-full">
          <div class="flex flex-col w-full h-full gap-2 justify-center items-center">
            <p class="text-md sm:text-2xl text-gray-500">ยังไม่มีคำศัพท์ โปรดเพิ่มคำศัพท์ก่อน</p>
            <button @click="toggleAddWord"
                    class="bg-green-400 hover:bg-green-600 text-white text-md sm:text-xl w-36 h-10 rounded">
              เพิ่มคำศัพท์
            </button>
          </div>
        </div>

        <div v-if="isAddWord" class="flex flex-col gap-2 justify-center items-center">
          <div class="mb-10">
            <p class="font-bold text-2xl sm:text-6xl text-gray-500">เพิ่มการ์ดคำศัพท์</p>
          </div>
          <div class="flex flex-col">
            <label for="eng" class="font-bold sm:text-lg">คำศัพท์ภาษาอังกฤษ</label>
            <input id="eng" @change="(e) => checkWords(e)"
                   class="w-72 h-10 pl-1 border-2 border-gray-200 rounded-md text-md focus:outline-blue-300 "
                   placeholder=" ใส่คำศัพท์ภาษาอังกฤษ"/>
            <p v-if="errorMsg.engWord" class="mt-2 text-md text-red-400">ต้องเป็นคำศัพท์ภาษาอังกฤษเท่านั้น</p>
          </div>
          <div class="flex flex-col">
            <label for="th" class="font-bold sm:text-lg">คำศัพท์ภาษาไทย</label>
            <input id="th" @change="(e) => checkWords(e)"
                   class="w-72 h-10 pl-1 border-2 border-gray-200 rounded-md text-md focus:outline-blue-300 "
                   placeholder=" ใส่คำศัพท์ภาษาไทย"/>
            <p v-if="errorMsg.thaiWord" class="mt-2 text-md text-red-400">ต้องเป็นคำศัพท์ภาษาไทยเท่านั้น</p>
          </div>
          <div class="flex gap-4 mt-10">
            <button :disabled="!(errorMsg.thaiWord === false && errorMsg.engWord === false)"
                    @click="handleSubmitAddWord"
                    :class="`text-white text-md sm:text-xl w-24 h-8 rounded ` + ((errorMsg.thaiWord === false && errorMsg.engWord === false ) ? `bg-green-400 hover:bg-green-600`: 'bg-gray-500') ">
              ยืนยัน
            </button>
            <button @click="isAddWord = !isAddWord"
                    class="bg-orange-400 hover:bg-orange-600 text-white text-md sm:text-xl w-24 h-8 rounded">
              ยกเลิก
            </button>
          </div>
        </div>

        <div v-if="(isSelectMode === false && data.length >= 1)" class="w-full h-full">
          <div v-if="(!isAddWord && data.length > 0)"
               class="flex w-full h-full flex-col gap-2 justify-center items-center gap-3">
            <div class="flex w-full h-1/6 justify-center items-center pt-3 ">
              <p class="text-md text-2xl sm:text-4xl text-gray-500">การ์ดคำศัพท์</p>
            </div>

            <div class="flex flex-row w-full h-4/6 justify-center items-center -space-x-52 flex-row ">
              <div v-for="word in data">
                <Card :data="word"/>
              </div>
            </div>

            <div class="flex flex-col w-full h-1/6 justify-center items-center m-auto ">
              <p v-if="data.length < 5" class="text-md sm:text-lg text-yellow-500 mt-3">ต้องมีการ์ดอย่างน้อง 5 ใบ</p>
              <button v-if="data.length < 5" @click="toggleAddWord"
                      class="bg-green-400 hover:bg-green-600 text-white text-md sm:text-xl w-36 h-10 rounded">
                เพิ่มคำศัพท์
              </button>
              <button v-if="data.length >= 5" @click="toggleGameMode"
                      class="bg-green-400 hover:bg-green-600 text-white text-md sm:text-xl w-36 h-10 rounded">
                เริ่มเกม
              </button>
            </div>
          </div>
        </div>

        <div v-if="!isSelectMode === false && isGameStart === false" class="w-full h-full">
          <div class="flex w-full h-1/6 justify-center items-center pt-3 ">
            <p class="text-md text-2xl sm:text-4xl text-gray-500">เลือกรูปแบบเกม</p>
          </div>

          <div class="flex flex-col sm:flex-row w-full h-4/6 justify-center items-center gap-5">
            <div @click="gameMode = 'th'"
                 :class="`flex justify-center items-center text-white text-md sm:text-xl w-36 h-24 rounded hover:cursor-pointer ${(gameMode === 'th' ? ' bg-pink-400' : ' bg-pink-200')}`">
              <p>ทายภาษาไทย</p>
            </div>
            <div @click="gameMode = 'en'"
                 :class="`flex justify-center items-center text-white text-md sm:text-xl w-36 h-24 rounded hover:cursor-pointer ${(gameMode === 'en' ? ' bg-violet-400' : ' bg-violet-200')}`">
              <p>ทายภาษาอังกฤษ</p>
            </div>
          </div>

          <div class="flex flex-row w-full h-1/6 justify-center items-center m-auto">
            <button v-if="data.length >= 3" @click="toggleGameStart"
                    :disabled="!(gameMode !== '')"
                    :class="`text-white text-md sm:text-xl w-36 h-10 rounded ${(gameMode !== '') ? ' bg-blue-400 hover:bg-blue-600' : ' bg-gray-500'}`">
              เริ่มเกม
            </button>
          </div>
        </div>

        <div v-if="isGameStart === true" class="flex flex-col w-full h-full">
          <div class="flex w-full h-1/2 justify-center items-center">
            <Card :mode="gameMode" :data="randomCard()"/>
          </div>
          <div class="flex justify-center items-center w-full h-1/2">
            <div class="flex flex-wrap  justify-center items-center gap-3">
              <div v-for="(word , index) in arrayOfAns" @click="ans = (gameMode === 'en' ? word.en :word.th)"
                   :class="`flex justify-center items-center text-white text-md sm:text-xl w-28 h-12 rounded hover:cursor-pointer ${(ans === (gameMode === 'en' ? word.en : word.th) ? ' bg-pink-400' : ' bg-pink-200')}`">
                <p>{{ (gameMode === "en" ? word.en : word.th) }}</p>
              </div>
            </div>
          </div>
          <div class="flex flex-row w-full h-1/6 justify-center items-center m-auto">
            <button @click="checkAns"
                    class="text-white text-md sm:text-xl w-36 h-10 rounded bg-red-400 hover:bg-red-600">
              ตรวจคำตอบ
            </button>
          </div>

        </div>
      </div>

      <div v-if="isCheckAns" class="absolute w-full h-full bg-black bg-opacity-50">
        <div class="flex flex-col w-full h-full ">
          <div class="flex flex-col w-full h-full justify-center items-center">
            <p :class="`text-7xl sm:text-9xl font-bold ${checkAns() ? 'text-green-400' : 'text-red-400'}`">{{checkAns() ? 'ถูกต้อง' : 'ผิด'}}</p>
            <p v-if="!checkAns()" class="text-3xl sm:text-5xl font-bold text-yellow-300">คำตอบคือ {{  (gameMode === 'en' ? question.en : question.th)}}</p>
            <div class="flex mt-5 justify-center items-center gap-3">
              <button @click="restartGame"
                      class="text-white text-md sm:text-xl w-28 sm:w-36 h-10 rounded bg-blue-400 hover:bg-red-600">
                เล่นอีกครั้ง
              </button>
              <button @click="cancelGame"
                      class="text-white text-md sm:text-xl w-28 sm:w-36 h-10 rounded bg-orange-400 hover:bg-red-600">
                กลับไปหน้าหลัก
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
