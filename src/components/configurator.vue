<script setup>
  import { ref, onMounted, provide } from 'vue'
  import ConfiguratorLayer from './configurator-layer.vue'


  const settings = ref({
    layerWidth: 500,
    layerHeight: 600,
    debug: false,
  })

  const configurations = ref([
    {
      id: 1,
      name: 'model 1',
      description: 'Description 1',
      case: 'https://picsum.photos/500/600?v=1',
      ring: 'https://picsum.photos/200/200?v=1',
    },
    {
      id: 2,
      name: 'model 2',
      description: 'Description 2',
      case: 'https://picsum.photos/500/600?v=2',
      ring: 'https://picsum.photos/200/200?v=2',
    },
    {
      id: 3,
      name: 'model 3',
      description: 'Description 3',
      case: 'https://picsum.photos/500/600?v=3',
      ring: 'https://picsum.photos/200/200?v=3',
    },

  ])

  const configuration = ref({
    case: 1,
    ring: 1,
  })

  const setLayer = (layer, value) => {
    configuration.value[layer] = value;
  }

  onMounted(() => {
    const urlParams = new URLSearchParams(window.location.search);
    if (urlParams.has('debug')) {
      settings.value.debug = true;
    }
  })

  provide('settings', settings)
  provide('configurations', configurations)
  provide('configuration', configuration)
</script>

<template>
  <div class="container configurator d-flex flex-column align-items-center justify-content-center"  :class="{ 'debug': settings.debug }">
    <pre v-if="settings.debug">{{ configuration }}</pre>
    <h1>Configurator</h1>
    <div class="layers" :style="{ width: settings.layerWidth + 'px', height: settings.layerHeight + 'px' }">
      <ConfiguratorLayer name="ring" />  
      <ConfiguratorLayer name="case" />    
    </div>
    <div class="controls">
      <div class="control case m-1">
        <label for="case" class="px-2">Case</label>
        <button class="btn btn-light prev mx-1" @click="setLayer('case', (configuration.case - 1) < 1 ? configurations.length : configuration.case - 1)">Prev</button>
        <button class="btn btn-light next mx-1" @click="setLayer('case', (configuration.case + 1) > configurations.length ? 1 : configuration.case + 1)">Next</button>
        <div class="info px-2 d-inline">{{ configuration.case }}</div>
      </div>
      <div class="control ring m-1">
        <label for="ring" class="px-2">Ring</label>
        <button v-for="c in configurations" class="btn mx-1" :class="configuration.ring === c.id ? 'btn-primary' : 'btn-light'" @click="setLayer('ring', c.id)">{{ c.name }}</button>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>

</style>

<style lang="scss" >
  .configurator {
    padding: 0;
    margin: 0;
    .layers {
      position: relative;
      overflow: hidden;
      .layer {
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        transition: transform 0.3s ease-in-out;
        //transition: transform 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55);
        &.ring {
          z-index: 1;
        }
        &.case {
          z-index: 0;
        }
        
      }
    }
    .controls {
      bottom: 0;
      text-align: center;
      .control {
        text-align: center;
      }
      button {
        &.active {
          background-color: red;
        }
      }
    }
  
    &.debug { // debug styling
      border: 1px solid blue;
      .layers {
        overflow: visible;
        .layer {
          border: 1px solid red;
          &.ring {
            img {
              opacity: 0.5;
            }
          }
        }
      }
      .controls {
        border: 1px solid green;
      }
    }
  }

</style>
