<template>
    <div
      class="with-transition hover:from-indigo-400 hover:to-basePurple hover:text-white group rounded-lg relative border p-2.5 flex flex-col space-y-2 group hover:shadow-xl hover:shadow-indigo-500/20 bg-gradient-to-br transition cursor-pointer"
    >
      <div class="inline-flex items-center space-x-2 text-sm">
        <span class="truncate capitalize">{{ language.batak }}</span>
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
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 60 60" width="24px" height="24px" @click="toggleSpeaking">
            <g stroke="#fff" stroke-linecap="round" stroke-linejoin="round" stroke-width="0">
                <g id="SVGRepo_bgCarrier"></g>
                <g id="SVGRepo_tracerCarrier"></g>
                <g id="SVGRepo_iconCarrier">
                <path d="M45.563 29.174l-22-15c-.307-.208-.703-.231-1.031-.058C22.205 14.289 22 14.629 22 15v30c0 .371.205.711.533.884.168.115.365.174.563.174.16 0 .321-.038.467-.115l22-15c.273-.166.437-.475.437-.816s-.164-.65-.437-.816l-22-15c-.146-.077-.307-.115-.467-.115-.223 0-.444.077-.611.216-.328.173-.533.513-.533.884v30c0 .371.205.711.533.884.168.115.365.174.563.174.16 0 .321-.038.467-.115l22-15c.273-.166.437-.475.437-.816s-.164-.65-.437-.816z" fill="#fff" />
                <path d="M30 0C13.458 0 0 13.458 0 30s13.458 30 30 30 30-13.458 30-30S46.542 0 30 0zm0 58C14.561 58 2 45.439 2 30S14.561 2 30 2s28 12.561 28 28-12.561 28-28 28z" fill="#fff" />
                </g>
            </g>
        </svg>

    </div>
  </template>

  <style scoped>
  .inline-flex {
    display: inline-flex;
    align-items: center;
  }
  
  svg {
    position: absolute;
    top: 0;
    right: 0;
    margin: 5px;
  }
</style>
  
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
      speak(`${language.english} in Batak Toba is ${language.batak}`);
    }
    speaking.value = synth.speaking;
  }
  
  </script>
  