<script>
import * as PIXI from 'pixi.js'
const hornsData = require(`../../public/rg/horns.json`)
const backgroundData = require(`../../public/rg/background.json`)

export default {
    data() {
        return {
            sprites: [
                {
                    id: 'horns1',
                    data: hornsData,
                    loc: {
                        x: 100,
                        y: 100
                    }
                },
                {
                    id: 'horns2',
                    data: hornsData,
                    loc: {
                        x: 300,
                        y: 300
                    }
                }
            ]
        }
    },
    mounted() {

        let app = new PIXI.Application({ resizeTo: window });
        this.$refs.canvas.appendChild(app.view);

        let background//, horns

        const backgroundSpritesheet = new PIXI.Spritesheet(
            PIXI.BaseTexture.from(`../../rg/${backgroundData.meta.image}`),
            backgroundData
        );

        backgroundSpritesheet.parse((sprites) => {
            background = new PIXI.Sprite(sprites["background.png"]); 
            app.stage.addChild(background);

            // scale stage container that it fits into the view
            app.stage.scale.x = app.view.width / background.width;
            app.stage.scale.y = app.view.height / background.height;
        })

        this.sprites.forEach( sprite => {

            // Create the SpriteSheet from data and image
            const hornsSpritesheet = new PIXI.Spritesheet(
                PIXI.BaseTexture.from(`../../rg/${sprite.data.meta.image}`),
                sprite.data
            );

            

            
            // Generate all the Textures asynchronously
            hornsSpritesheet.parse(() => {
                
                let horns = new PIXI.AnimatedSprite(hornsSpritesheet.animations.horns);
                horns.interactive = true
                // set speed, start playback and add it to the stage
                horns.animationSpeed = 0.167;
                horns.position.set(sprite.loc.x, sprite.loc.y); // almost bottom-left corner of the canvas
                app.stage.addChild(horns);

                horns
                .on('pointerover', onButtonDown)
                .on('pointerout', onButtonUp);

                function onButtonDown() {
                    horns.play()
                }

                function onButtonUp() {
                    horns.stop()
                }

            });

            
        
        })

        
        
    }

    
}
</script>

<template>
    <div ref="canvas"></div>
</template>