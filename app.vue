<template>
  <div class="bg-slate-100 flex items-start">
    <div
      class="bg-white 2xl:p-6 xl:p-0 rounded-lg shadow-lg 2xl:w-1/3 xl:w-1/2 lg:w-full max-w-5xl flex min-h-screen"
    >
      <div class="flex-1 p-4">
        <h1 class="text-4xl font-bold text-gradient mb-4 text-center">
          Gradient Text Generator
        </h1>
        <p class="text-slate-500 mb-4 text-center">
          Instantly create and embed stunning gradient texts using CSS.<br />
          Simple, efficient, and perfect for developers looking to enhance their
          projects.
        </p>
        <div class="2xl:hidden lg:block">
          <Sample
            :copyCSS="copyCSS"
            :text="text"
            :gradientTextStyle="gradientTextStyle"
            :clicked="clicked"
          />
        </div>
        <div>
          <label class="block text-slate-500 font-bold text-xl mb-2">
            Text
          </label>
          <div class="bg-slate-100 p-4 w-full rounded-xl mb-6 shadow-md">
            <input
              type="text"
              v-model="text"
              placeholder="Enter text"
              class="border-none bg-inherit text-slate-500 font-bold text-xl w-full outline-none"
            />
          </div>
        </div>
        <div>
          <label class="block text-slate-500 font-bold text-xl">Angle</label>
          <div class="flex justify-between items-center">
            <input
              id="myRange"
              class="slider shadow-md"
              v-model="angle"
              max="360"
              min="0"
              type="range"
            />
            <div
              class="inline-block bg-slate-100 p-2 ml-2 rounded-lg shadow-md"
            >
              <span class="text-xl">{{ angle }}°</span>
            </div>
          </div>
        </div>
        <div class="mt-4">
          <label class="block text-slate-500 font-bold text-xl mb-2"
            >Colors</label
          >
          <div
            v-for="(color, index) in colors"
            :key="index"
            class="flex items-center mb-2"
          >
            <input
              type="color"
              v-model="colors[index]"
              class="w-16 h-12 mr-2 bg-inherit cursor-pointer"
            />
            <input
              type="text"
              v-model="colors[index]"
              class="w-full p-2 border text-white text-center outline-none rounded-xl shadow-md"
              :style="{ background: colors[index] }"
              disabled
            />
            <button
              @click="removeColor(index)"
              class="ml-2 bg-slate-100 px-4 py-2 rounded-lg shadow-md"
              :class="{
                'text-red-500': colors.length > 2,
                'cursor-not-allowed': colors.length <= 2,
              }"
            >
              <IconsDelete :colors="colors" />
            </button>
          </div>
          <button
            @click="addColor"
            class="text-blue-500 mt-2 shadow-md flex justify-center items-center w-full px-2 py-4 rounded-lg gap-2"
          >
            <IconsAdd />
            NEW COLOR
          </button>
        </div>
      </div>
    </div>
    <div
      class="2xl:flex lg:hidden flex-1 flex-col items-center justify-center p-4 h-screen gap-5"
    >
      <Sample
        :copyCSS="copyCSS"
        :text="text"
        :gradientTextStyle="gradientTextStyle"
        :clicked="clicked"
      />
      <Code
        :angle="angle"
        :colorsString="colorsString"
        :copyCSS="copyCSS"
        :clicked="clicked"
      />
    </div>
  </div>
</template>

<script setup>
const text = ref("Sample Text");
const angle = ref(135);
const colors = ref(["#42D392", "#647EFF"]);

const clicked = ref(true);

const gradientTextStyle = computed(() => ({
  background: `linear-gradient(${angle.value}deg, ${colors.value.join(", ")})`,
  "-webkit-background-clip": "text",
  "-webkit-text-fill-color": "transparent",
}));

const colorsString = computed(() => colors.value.join(", "));

const addColor = () => {
  colors.value.push("#000000"); // default black color for new entry
};

const removeColor = (index) => {
  if (colors.value.length <= 2) {
    return;
  }
  colors.value.splice(index, 1);
};

const copyCSS = () => {
  clicked.value = !clicked.value;
  setTimeout(() => {
    clicked.value = !clicked.value;
  }, 2000);

  const css = `
background: linear-gradient(${angle.value}deg, ${colorsString.value});
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
  `;
  navigator.clipboard.writeText(css.trim());
};
</script>

<style scoped>
.text-gradient {
  background: linear-gradient(135deg, #42d392, #647eff);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.slider {
  -webkit-appearance: none;
  appearance: none;
  width: 90%;
  height: 10px;
  border-radius: 5px;
  background-color: #4158d0;
  background-image: linear-gradient(
    43deg,
    #4158d0 0%,
    #c850c0 46%,
    #ffcc70 100%
  );
  outline: none;
  opacity: 0.7;
  -webkit-transition: 0.2s;
  transition: opacity 0.2s;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background-color: #4c00ff;
  background-image: linear-gradient(160deg, #4900f5 0%, #80d0c7 100%);
  cursor: pointer;
}

.slider::-moz-range-thumb {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background-color: #0093e9;
  background-image: linear-gradient(160deg, #0093e9 0%, #80d0c7 100%);
  cursor: pointer;
}
</style>
