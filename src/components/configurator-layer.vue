<script setup>
import { ref, onMounted, defineProps, inject } from "vue";

const props = defineProps({
  name: {
      type: String,
      required: true
  }, 

})

const settings = inject('settings')
const configurations = inject('configurations')
const configuration = inject('configuration')


</script>

<template>
    <div :class="`layer ${name}`" :style="{ transform: `translateX(${(configuration[name] - 1) * -settings.layerWidth}px)` }">
      <div v-for="c in configurations" class="case-image">
        <img :src="c[name]" alt="ring" :style="{ width: settings.layerWidth + 'px' }" class="d-block">
      </div>
    </div>
</template>