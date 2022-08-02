<script>
import * as PIXI from 'pixi.js'
const spritesheetData = require(`../../public/capguy-spritesheets/spritesheet.json`)

export default {

    mounted() {

        let app = new PIXI.Application({ width: 800, height: 450 });
        this.$refs.canvas.appendChild(app.view);

        let background, animatedCapguy, ticker;

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
            console.log("anim", animatedCapguy)
            animatedCapguy.interactive = true
            // set speed, start playback and add it to the stage
            animatedCapguy.animationSpeed = 0.167;
            animatedCapguy.position.set(0, background.height - 350); // almost bottom-left corner of the canvas
            app.stage.addChild(animatedCapguy);

            animatedCapguy.play()

            //app.ticker.add(delta => gameLoop(delta));

            ticker = new PIXI.Ticker
            ticker.add(delta => gameLoop(delta));
            ticker.start()

            animatedCapguy.onFrameChange = function (x) {
                if (x === 3) animatedCapguy.gotoAndPlay(0)
            };

            animatedCapguy
            .on('pointerover', onButtonDown)
            .on('pointerout', onButtonUp);
        });

        function onButtonDown() {
            animatedCapguy.onFrameChange = function (x) {
                if (x === 7) animatedCapguy.gotoAndPlay(4)
            };
            ticker.stop()
        }

        function onButtonUp() {
            animatedCapguy.onFrameChange = function (x) {
                if (x === 3) animatedCapguy.gotoAndPlay(0)
            };
            ticker.start()
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