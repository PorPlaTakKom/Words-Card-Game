[1/31 12:13 PM] SIT LX1003


computedFilteredSearch.vue


JavaScript


<script setup>
import {

​ ref, computed
}​ from
'vue'
import IcOutlineSearch from './components/icons/IcOutlineSearch.vue'
import groupworks from './data/groupworks.json'

console.log(groupworks)
const groups = groupworks
const filterGroupWorks = computed(() => {​
    return groups.filter((group) =>
        group.projectName.toLowerCase().includes(searchKeyword.value.toLowerCase())
    )
}​)
const courseTitle =
    '<span class="text-orange-500">INT203-Client Side Programming II</span>'
const searchDisabled = false
const getFooter = (type) => {​
    let footer = 'School of Information Technology, KMUTT'
  if (type.toLowerCase() === 'long')
    footer = `INT203-Client Side Programming II, ${​footer}​`
  return footer
}​
const courseId = 'INT201'
const notActiveData = 1
const activeData = ref(1)
const incrementReactiveData = () => {​
  // return (activeData.value = activeData.value + 1)
  return ++activeData.value
}​
const searchKeyword = ref('')
const agree = ref(false)
const searchVisible = ref(false)
const toggleSearch = () => {​
    console.log('toggle calling...')
  console.log(searchVisible.value)
  searchVisible.value = !searchVisible.value
}​
</script>
<template>
  <div class="w-full">
    <div class="flex flex-col">
      <div class="flex items-center space-x-3">
        <input type="checkbox" id="agree" v-model="agree"/>
        <label for="checkbox">
          I <span>{​{​ agree ? 'agree' : 'not agree' }​}​</span> the terms and use
        </label>
      </div>
      <div class="flex items-center space-x-3">
        <p class="text-lg font-semibold">
          Not ReActive Data:{​{​ notActiveData }​}​
        </p>
        <button
            v-on:click="++notActiveData"
            class="border border-gray-300 rounded-lg"
        >
          +
        </button>
      </div>
      <div class="flex items-center space-x-3">
        <p class="text-lg font-semibold">ReActive Data:{​{​ activeData }​}​</p>
        <button
            v-on:click="incrementReactiveData"
            class="border border-gray-300 rounded-lg"
        >
          +
        </button>
      </div>
    </div>
    <div class="w-full flex-col">
      <!-- column#1 (header) -->
      <div class="w-full flex flex-row">
        <img src="./assets/vuelogo.ico" class="w-14 h-12 m-3"/>
        <div class="w-full flex flex-col p-2">
          <h1 class="tracking-widest text-emerald-500 text-xl">
            Vue3 Group Projects
          </h1>
          <h3 class="italic" v-html="courseTitle"></h3>
        </div>
        <div class="w-full flex p-2 m-2 justify-end">
          <IcOutlineSearch @click="toggleSearch"/>
          <div class="w-3/5 space-x-2 flex items-center" v-show="searchVisible">
            <input
                v-model.trim="searchKeyword"
                class="w-full p-1 outline-none rounded-lg border border-gray-200"
                type="text"
                placeholder="Type your keyword here..."
            />
            {​{​ searchKeyword }​}​
            <button
                class="border p-1 rounded-lg"
                :class="searchDisabled ? 'border-red-500' : 'border-green-500'"
            >
              Search
            </button>
          </div>
        </div>
      </div>
      <!-- column#2 (table) -->
      <div class="w-full mt-5 p-5">
        <table class="w-full">
          <thead class="bg-gray-50 border-b-2 border-gray-200">
          <tr>
            <th class="text-left font-semibold tracking-widest p-3">
              Section
            </th>
            <th class="text-left font-semibold tracking-widest p-3">
              Project Name
            </th>
            <th class="text-left font-semibold tracking-widest p-3">
              GitHub Repository
            </th>
            <th class="text-left font-semibold tracking-widest p-3">
              Group Members
            </th>
          </tr>
          </thead>
          <tbody>
          <tr
              :class="index % 2 === 0 ? 'bg-white' : 'bg-gray-100'"
              v-for="(groupwork, index) in filterGroupWorks"
              :key="index"
          >
            <td>{​{​ groupwork.section }​}​</td>
            <td>{​{​ groupwork.projectName }​}​</td>
            <td>
              <a
                  class="text-purple-800"
                  :href="groupwork.githubRepo"
                  target="_blank"
              >{​{​ groupwork.githubRepo }​}​</a
              >
            </td>
            <td>
              <div v-for="student in groupwork.students" :key="student.id">
                <p>{​{​ student.id }​}​ {​{​ student.name }​}​</p>
              </div>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
      <!-- column#3 (footer) -->
      <div class="w-full flex justify-center">
        {​{​ getFooter('short') }​}​
      </div>
    </div>
  </div>
</template>
<style scoped></style>