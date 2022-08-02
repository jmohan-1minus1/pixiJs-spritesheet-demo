<template>
<div>
    Basic sprite from raw .png's
    <BasicSprite/>
    Animation on hover, spritesheet from TexturePacker https://www.codeandweb.com/texturepacker
    <BasicSpritesheet/>
    Spritesheet with multiple animation on hover
    <HornsSpritesheet/>
    Multiple animations. Hover to change animation.
    <MultipleAnimations/>
  </div>
</template>

<script>
import BasicSprite from './components/BasicSprite.vue'
import BasicSpritesheet from './components/BasicSpritesheet.vue'
import HornsSpritesheet from './components/HornsSpritesheet.vue'
import MultipleAnimations from './components/MultipleAnimations.vue'

export default {
  name: 'App',
  components: {
    BasicSprite,
    BasicSpritesheet,
    HornsSpritesheet,
    MultipleAnimations
  }
}
</script>

<style scoped>
* {
  text-align: center;
}
</style>


