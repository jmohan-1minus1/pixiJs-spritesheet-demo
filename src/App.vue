<template>
  Tutorial
  <PixiScene/>
  Spritesheet + hover
  <PixiSceneOptimised/>
  Horns
  <PixiHorns/>
  Stacked animations
  <PixiStackedAnimations/>
</template>

<script>
import PixiScene from './components/PixiScene.vue'
import PixiSceneOptimised from './components/PixiSceneOptimised.vue'
import PixiHorns from './components/PixiHorns.vue'
import PixiStackedAnimations from './components/PixiStackedAnimations.vue'

export default {
  name: 'App',
  components: {
    PixiScene,
    PixiSceneOptimised,
    PixiHorns,
    PixiStackedAnimations
  }
}
</script>


