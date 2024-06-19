<template>
  <div class="min-h-screen bg-slate-100 flex items-start">
    <div
      class="bg-white p-6 rounded-lg shadow-lg w-1/3 max-w-5xl flex h-screen"
    >
      <div class="flex-1 p-4">
        <h1 class="text-4xl font-bold text-gradient mb-4 text-center">
          Gradient Text Generator
        </h1>
        <p class="text-slate-500 mb-4 text-center">
          Instantly create and embed stunning gradient texts using CSS or
          HTML.<br />
          Simple, efficient, and perfect for developers looking to enhance their
          projects.
        </p>
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
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="32"
                height="32"
                viewBox="0 0 24 24"
                :class="{ 'cursor-not-allowed': colors.length <= 2 }"
              >
                <path
                  fill="currentColor"
                  d="M7 21q-.825 0-1.412-.587T5 19V6H4V4h5V3h6v1h5v2h-1v13q0 .825-.587 1.413T17 21zm2-4h2V8H9zm4 0h2V8h-2z"
                />
              </svg>
            </button>
          </div>
          <button
            @click="addColor"
            class="text-blue-500 mt-2 shadow-md flex justify-center items-center w-full px-2 py-4 rounded-lg gap-2"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="32"
              height="32"
              viewBox="0 0 24 24"
            >
              <path
                fill="currentColor"
                d="M11 17h2v-4h4v-2h-4V7h-2v4H7v2h4zm1 5q-2.075 0-3.9-.788t-3.175-2.137T2.788 15.9T2 12t.788-3.9t2.137-3.175T8.1 2.788T12 2t3.9.788t3.175 2.137T21.213 8.1T22 12t-.788 3.9t-2.137 3.175t-3.175 2.138T12 22m0-2q3.35 0 5.675-2.325T20 12t-2.325-5.675T12 4T6.325 6.325T4 12t2.325 5.675T12 20m0-8"
              />
            </svg>
            NEW COLOR
          </button>
        </div>
      </div>
    </div>
    <div
      class="flex-1 flex flex-col items-center justify-center p-4 h-screen gap-5"
    >
      <div
        class="bg-white px-20 py-10 text-4xl rounded-2xl shadow-lg text-center"
      >
        <div
          class="text-6xl font-black text-center uppercase"
          :style="gradientTextStyle"
        >
          {{ text }}
        </div>
      </div>
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
