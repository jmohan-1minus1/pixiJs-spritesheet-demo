<script>
import * as PIXI from 'pixi.js'
const spritesheetData = require(`../../public/capguy-spritesheets/spritesheet.json`)

export default {

    mounted() {

        let app = new PIXI.Application({ width: 800, height: 450 });
        this.$refs.canvas.appendChild(app.view);

        let background, animatedCapguy;

        // Create the SpriteSheet from data and image
        const spritesheet = new PIXI.Spritesheet(
            PIXI.BaseTexture.from(`../../capguy-spritesheets/${spritesheetData.meta.image}`),
            spritesheetData
        );

        
        // Generate all the Textures asynchronously
        spritesheet.parse((sprites) => {

            background = new PIXI.Sprite(sprites["background.png"]); 
            app.stage.addChild(background);

            // scale stage container that it fits into the view
            app.stage.scale.x = app.view.width / background.width;
            app.stage.scale.y = app.view.height / background.height;
            
            animatedCapguy = new PIXI.AnimatedSprite(spritesheet.animations.walk);
            animatedCapguy.interactive = true
            // set speed, start playback and add it to the stage
            animatedCapguy.animationSpeed = 0.167;
            animatedCapguy.position.set(0, background.height - 350); // almost bottom-left corner of the canvas
            app.stage.addChild(animatedCapguy);

            animatedCapguy
            .on('pointerover', onButtonDown)
            .on('pointerout', onButtonUp);
        });

        function onButtonDown() {
            animatedCapguy.play()
        }

        function onButtonUp() {
            animatedCapguy.stop()
        }
        
    }

    
}
</script>

<template>
    <div ref="canvas"></div>
</template>