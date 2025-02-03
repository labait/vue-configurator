<script setup>
  import { ref, onMounted } from 'vue'

  const layerWidth = 500;
  const layerHeight = 600;
  const debug = ref(false);

  const configurations = ref([
    {
      id: 1,
      name: 'Configuration 1',
      description: 'Description 1',
      case: 'https://picsum.photos/500/600?v=1',
      ring: 'https://picsum.photos/200/200?v=1',
    },
    {
      id: 2,
      name: 'Configuration 2',
      description: 'Description 2',
      case: 'https://picsum.photos/500/600?v=2',
      ring: 'https://picsum.photos/200/200?v=2',
    },
    {
      id: 3,
      name: 'Configuration 3',
      description: 'Description 3',
      case: 'https://picsum.photos/500/600?v=3',
      ring: 'https://picsum.photos/200/200?v=3',
    },

  ])

  const configuration = ref({
    case: 1,
  })

  const setLayer = (layer, value) => {
    configuration.value[layer] = value;
  }

  onMounted(() => {
    const urlParams = new URLSearchParams(window.location.search);
    if (urlParams.has('debug')) {
      debug.value = true;
    }
  })
</script>

<template>
  <div class="container configurator d-flex flex-column align-items-center justify-content-center"  :class="{ 'debug': debug }">
    <pre v-if="debug">{{ configuration }}</pre>
    <h1>Configurator</h1>
    <div class="layers" :style="{ width: layerWidth + 'px', height: layerHeight + 'px' }">
      <div class="layer lancet">
      
      </div>
      <div class="layer quadrant">
      
      </div>
      <div class="layer ring">
        <div class="layer ring" :style="{ transform: `translateX(${(configuration.ring - 1) * -layerWidth}px)` }">
          <div v-for="c in configurations" class="case-image">
            <img :src="c.ring" alt="ring" :style="{ width: layerWidth + 'px' }" class="d-block">
          </div>
        </div>
      </div>
      <div class="layer case" :style="{ transform: `translateX(${(configuration.case - 1) * -layerWidth}px)` }">
        <div v-for="c in configurations" class="case-image">
          <img :src="c.case" alt="case" :style="{ width: layerWidth + 'px' }" class="d-block">
        </div>
      </div>
    </div>
    <div class="controls">
      <div class="control case">
        <button class="prev" @click="setLayer('case', (configuration.case - 1) < 1 ? configurations.length : configuration.case - 1)">Prev</button>
        <button class="next" @click="setLayer('case', (configuration.case + 1) > configurations.length ? 1 : configuration.case + 1)">Next</button>
      </div>
      <div class="control ring">
        <button @click="setLayer('ring', 1)">ring1</button>
        <button @click="setLayer('ring', 2)">ring2</button>
        <button @click="setLayer('ring', 3)">ring3</button>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
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
        transition: transform 0.5s ease-in-out;
        .controls {
          position: absolute;
          bottom: -50px;
          width: 100%;
          text-align: center;
        }
        &.ring {
          z-index: 1;
        }
        &.case {
          z-index: 0;
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
