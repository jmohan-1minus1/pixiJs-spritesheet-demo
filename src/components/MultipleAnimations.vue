<script>
import * as PIXI from 'pixi.js'
const spritesheetData = require(`../../public/capguy_reversed/spritesheet.json`)

export default {

    mounted() {

        let app = new PIXI.Application({ width: 800, height: 450 });
        this.$refs.canvas.appendChild(app.view);

        let background, animatedCapguy//, ticker;

        // Create the SpriteSheet from data and image
        const spritesheet = new PIXI.Spritesheet(
            PIXI.BaseTexture.from(`../../capguy_reversed/${spritesheetData.meta.image}`),
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
            // set speed, start playback and add it to the stage
            animatedCapguy.animationSpeed = 0.167;
            animatedCapguy.position.set(0, background.height - 350); // almost bottom-left corner of the canvas
            app.stage.addChild(animatedCapguy);
            animatedCapguy.play()

            // loop through first 8 frames of animation
            animatedCapguy.onFrameChange = function (x) {
                if (x === 7) animatedCapguy.gotoAndPlay(0)
            };

            // interactivity
            animatedCapguy.interactive = true
            animatedCapguy
            .on('pointerover', onButtonDown)
            .on('pointerout', onButtonUp);

            // loop
            app.ticker.add(delta => gameLoop(delta));
        });

        function onButtonDown() {
            // set range of animation to last 8 frames
            animatedCapguy.onFrameChange = function (x) {
                if (x === 15) animatedCapguy.gotoAndPlay(8)
            };
        }

        function onButtonUp() {
            // reset back to first 8 frames
            animatedCapguy.onFrameChange = function (x) {
                if (x === 7) animatedCapguy.gotoAndPlay(0)
            };
        }

        function gameLoop(delta) {
            animatedCapguy.x = (animatedCapguy.x + 5*delta) % (background.width + 200);
        }


        
        
    }

    
}
</script>

<template>
    <div ref="canvas"></div>
</template>