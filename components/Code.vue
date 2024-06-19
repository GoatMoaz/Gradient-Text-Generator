<template>
  <div>
    <div class="code-editor">
      <div class="header">
        <div v-if="clicked">
          <Copy @click="copyCSS" />
        </div>
        <div v-else>
          <Copied />
        </div>
      </div>
      <div class="editor-content">
        <code class="code">
          <p class="property">
            <span class="color-2">background</span><span>: </span>
            <span class="color-3">linear-gradient (</span>{{ angle }}deg,
            {{ colorsString }}<span class="color-3">)</span>
            <span>;</span>
          </p>
          <p class="property">
            <span class="color-2"> -webkit-background-clip</span>
            <span>: </span>
            <span class="color-3">text</span>
            <span>;</span>
          </p>
          <p class="property">
            <span class="color-2">-webkit-text-fill-color</span><span>: </span>
            <span class="color-3">transparent</span><span>;</span>
          </p>
        </code>
      </div>
    </div>
  </div>
</template>

<script setup>
const { angle, colorsString, copyCSS, clicked } = defineProps([
  "angle",
  "colorsString",
  "copyCSS",
  "clicked",
]);
</script>

<style  scoped>
.code-editor {
  max-width: 700px;
  background-color: #1d1e22;
  box-shadow: 0px 4px 30px rgba(0, 0, 0, 0.5);
  border-radius: 8px;
  padding: 2px 0 1.2rem 1.2rem;
}

.header {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin: 10px;
}

.editor-content {
  margin: 0 10px 10px;
  color: white;
}

.property:hover {
  cursor: text;
}

.editor-content .color-2 {
  color: rgb(156 220 254);
}

.editor-content .color-3 {
  color: rgb(207 146 120);
}
</style>