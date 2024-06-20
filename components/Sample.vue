<template>
  <div
    class="bg-white lg:bg-slate-100 pad text-4xl rounded-2xl shadow-lg text-center lg:mb-4"
  >
    <div class="lg:flex 2xl:hidden justify-end items-center">
        <div v-if="clicked">
          <IconsCopy @click="copyCSS" color="black" size="0.8em"/>
        </div>
        <div v-else>
            <IconsCopied size="0.8em" />
        </div>
    </div>
    <div
      class="text-6xl xl:text-4xl lg:text-3xl md:text-2xl sm:text-lg font-black text-center uppercase"
      :style="gradientTextStyle"
    >
      {{ text }}
    </div>
  </div>
</template>

<script setup>
const { text, gradientTextStyle, copyCSS, clicked } = defineProps([
  "text",
  "gradientTextStyle",
  "copyCSS",
  "clicked",
]);

</script>

<style scoped>
.pad {
  padding-left: 5rem;
  padding-right: 5rem;
  padding-top: 2.5rem;
  padding-bottom: 2.5rem;
}

@media screen and (max-width: 768px) {
  .pad {
    padding: 2px 0 1.2rem 1.2rem;
  }
}
</style>