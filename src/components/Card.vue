<script setup>
import {ref} from "vue";

defineProps({
  data: Object,
  mode: String
})

</script>

<template>
  <div class="group w-44 h-52">
    <div class="flex w-full h-full rounded-xl shadow-lg  overflow-hidden duration-200 group-hover:scale-105 group-hover:cursor-pointer">
      <div class="block w-full h-full">
        <div class="flex flex-col w-full h-full p-5 bg-violet-300 gap-1   ">
          <div class="flex w-full h-1/2 justify-center items-center ]">
            <p class="text-2xl text-white font-bold">
              {{ (mode === "en" ? '?' : data.en) }}

            </p>
          </div>
          <div class="flex w-full h-1/2 justify-center items-center">
            <p class="text-2xl text-white font-bold">
              {{ (mode === "th" ? '?' : data.th) }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>