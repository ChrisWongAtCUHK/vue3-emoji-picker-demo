<script setup>
import { ref } from "vue";
import { ChevronDownIcon } from "@heroicons/vue/24/outline";
import EmojiPicker from "vue3-emoji-picker";
import "vue3-emoji-picker/css";

const emojis = ref(["😀", "😂", "🔥", "❤️"]);
const sequence = ref([]);
const selectedEmoji = ref(null);
const selectedColors = ref(Array(emojis.value.length).fill("bg-[#4A4A4A]"));

const selectEmoji = (emoji, index) => {
  const existingIndex = sequence.value.findIndex((e) => e.emoji === emoji);

  if (existingIndex === -1) {
    sequence.value.push({
      emoji,
      order: sequence.value.length + 1,
    });
  } else {
    sequence.value.splice(existingIndex, 1);
  }
  for (let i = 0; i < sequence.value.length; i++) {
    console.log(sequence.value[i]);
  }

  selectedEmoji.value = index;
  selectedColors.value[index] =
    selectedColors.value[index] === "bg-[#4A4A4A]"
      ? "bg-[#25d3ff8a]"
      : "bg-[#4A4A4A]";
};

const changeEmoji = (index, newEmoji) => {
  emojis.value[index] = newEmoji;
};
</script>

<template>
  <div class="flex justify-center items-center h-300">
    <div class="grid grid-cols-2 gap-4 p-4 bg-[#4a4a4a] rounded-lg shadow-md">
      <div
        v-for="(emoji, index) in emojis"
        :key="index"
        class="relative w-64 h-64 flex justify-center items-center rounded-lg cursor-pointer hover:scale-105 transition"
        :class="selectedColors[index]"
        @click="selectEmoji(emoji, index)"
      >
        <span class="justify-center text-9xl text-white">{{ emoji }}</span>

        {{ console.log("Emoji:", emoji, "Sequence:", sequence) }}
        //This would show the user his current emoji sequence
        <span @click.stop="togglePicker(index)">
          <ChevronDownIcon class="rotate-180" />
        </span>
      </div>
      <EmojiPicker
        @select="(emoji) => changeEmoji(index, emoji.i)"
        class="absolute"
      />
    </div>
  </div>
</template>
