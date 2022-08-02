<script>
import * as PIXI from 'pixi.js'
const backgroundImg = require("@/assets/images/sprites/background.png")

export default {
    mounted() {

        // create a Pixi application
        let app = new PIXI.Application({ width: 800, height: 450 });

        // add the canvas that Pixi automatically created for you to the HTML document
        this.$refs.canvas.appendChild(app.view);

        let background, animatedCapguy;

        const capguyFrames = [
            require("@/assets/images/sprites/capguy/walk_01.png"),
            require("@/assets/images/sprites/capguy/walk_02.png"),
            require("@/assets/images/sprites/capguy/walk_03.png"),
            require("@/assets/images/sprites/capguy/walk_04.png"),
            require("@/assets/images/sprites/capguy/walk_05.png"),
            require("@/assets/images/sprites/capguy/walk_06.png"),
            require("@/assets/images/sprites/capguy/walk_07.png"),
            require("@/assets/images/sprites/capguy/walk_08.png")
        ];

        // load the sprite, call setup() when completed
        app.loader
            .add(backgroundImg)
            .add(capguyFrames)
            .load(setup);

        function setup() {
            let resources = app.loader.resources;

            // initialize background sprite
            background = new PIXI.Sprite(resources[backgroundImg].texture);
            app.stage.addChild(background);

            // scale stage container that it fits into the view
            app.stage.scale.x = app.view.width / background.width;
            app.stage.scale.y = app.view.height / background.height;

            // create an animated sprite
            animatedCapguy = new PIXI.AnimatedSprite.fromFrames(capguyFrames);
            // configure + start animation:
            animatedCapguy.animationSpeed = 1/6;                  // 6 fps
            animatedCapguy.position.set(0, background.height - 350); // almost bottom-left corner of the canvas
            animatedCapguy.play();
            app.stage.addChild(animatedCapguy);
            // add it to the stage and render!
            app.stage.addChild(animatedCapguy);
            app.ticker.add(delta => gameLoop(delta));
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