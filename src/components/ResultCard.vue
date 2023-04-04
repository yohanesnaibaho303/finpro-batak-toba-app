<template>
    <div
      class="with-transition hover:from-indigo-400 hover:to-basePurple hover:text-white group rounded-lg relative border p-2.5 flex flex-col space-y-2 group hover:shadow-xl hover:shadow-indigo-500/20 bg-gradient-to-br transition cursor-pointer"
    >
      <div class="inline-flex items-center space-x-2 text-sm">
        <span class="truncate capitalize">{{ language.batak }} <button @click="toggleSpeaking">{{ speaking ? 'Stop' : 'Speak' }}</button></span>
      </div>
  
      <div class="inline-flex items-center space-x-2">
        <p class="text-xs capitalize italic">
          <span class="text-gray-500 group-hover:text-white text-[10px]"
            >English:</span
          >
          {{ language.english }}
        </p>
      </div>
  
      <svg
        xmlns="http://www.w3.org/2000/svg"
        area-hidden="true"
        class="h-6 w-6 transform rotate-90 top-0 right-2 absolute text-white"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M15 5v2m0 4v2m0 4v2M5 5a2 2 0 00-2 2v3a2 2 0 110 4v3a2 2 0 002 2h14a2 2 0 002-2v-3a2 2 0 110-4V7a2 2 0 00-2-2H5z"
        />
      </svg>
    </div>
  </template>
  
  <script lang="ts" setup>
  import { ref } from "vue";
  
  import { defineProps } from "@vue/runtime-core";
  
  const { language, index } = defineProps<{
    language: { english: string; batak: string };
    index: number;
  }>();
  
  const query = ref("");
  const speaking = ref(false);
  
  // Inisialisasi speech synthesis
  const synth = window.speechSynthesis;
  
  // Fungsi untuk membaca teks
  function speak(text: string) {
    const utterance = new SpeechSynthesisUtterance(text);
    utterance.onstart = () => {
      speaking.value = true;
    };
    utterance.onend = () => {
      speaking.value = false;
    };
    synth.speak(utterance);
  }
  
  function stop() {
    synth.cancel();
    speaking.value = false;
  }
  
  function toggleSpeaking() {
    if (speaking.value) {
      stop();
    } else {
      speak(`${language.english} in Batak Toba is ${language.batak}`);
    }
  }
  
  </script>
  