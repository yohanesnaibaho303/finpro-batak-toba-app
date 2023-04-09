<template>
    <div
      class="card-container group"
    >
      <div class="card-batak-text">
        <svg v-if="index == 0" xmlns="http://www.w3.org/2000/svg" class="card-batak-svg" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M17.707 9.293a1 1 0 010 1.414l-7 7a1 1 0 01-1.414 0l-7-7A.997.997 0 012 10V5a3 3 0 013-3h5c.256 0 .512.098.707.293l7 7zM5 6a1 1 0 100-2 1 1 0 000 2z" clip-rule="evenodd" />
        </svg>
        <div class="inline-flex items-center space-x-2 text-sm">
          <span class="truncate capitalize">{{ language.batak }}</span>
        </div>
      </div>
  
      <div class="inline-flex items-center space-x-2">
        <p class="text-xs capitalize italic">
          <span class="text-gray-500 group-hover:text-white text-[10px]"
            >English:</span
          >
          {{ language.english }}
        </p>
      </div>
        
      <!-- icon untuk click speak -->
      <svg class="svgspeak" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" width="24px" height="24px" @click="toggleSpeaking" >
        <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
        <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
        <g id="SVGRepo_iconCarrier"> 
          <path fill-rule="evenodd" clip-rule="evenodd" d="M11 6.04029C11 4.78253 9.5451 4.08327 8.56296 4.86899L4.64922 7.99998H3C1.34315 7.99998 0 9.34312 0 11V13C0 14.6568 1.34315 16 3 16H4.64922L8.56296 19.131C9.5451 19.9167 11 19.2174 11 17.9597V6.04029ZM5.89861 9.56172L9 7.0806V16.9194L5.89861 14.4382C5.54398 14.1545 5.10336 14 4.64922 14H3C2.44772 14 2 13.5523 2 13V11C2 10.4477 2.44772 9.99998 3 9.99998H4.64922C5.10336 9.99998 5.54398 9.84542 5.89861 9.56172ZM17.0711 5.12124C16.6805 4.73072 16.0474 4.73072 15.6568 5.12124C15.2663 5.51177 15.2663 6.14493 15.6568 6.53545C18.781 9.65965 18.781 14.725 15.6568 17.8492C15.2663 18.2397 15.2663 18.8729 15.6568 19.2634C16.0474 19.6539 16.6805 19.6539 17.0711 19.2634C20.9763 15.3581 20.9763 9.02648 17.0711 5.12124ZM14.2426 7.94967C13.8521 7.55914 13.2189 7.55914 12.8284 7.94967C12.4379 8.34019 12.4379 8.97336 12.8284 9.36388C14.3905 10.926 14.3905 13.4586 12.8284 15.0207C12.4379 15.4113 12.4379 16.0444 12.8284 16.4349C13.2189 16.8255 13.8521 16.8255 14.2426 16.4349C16.5858 14.0918 16.5858 10.2928 14.2426 7.94967Z" fill="#ffffff"></path> 
        </g>
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
  
  
  function toggleSpeaking() {
    if (synth.speaking) {
      synth.cancel();
    } else {
      speak(`${language.batak}`);
    }
    speaking.value = synth.speaking;
  }

  
  </script>
  
  <!-- speak(`${language.english} in Batak Toba is ${language.batak}`); -->