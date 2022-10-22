<script setup>
import axios from "axios";
// eslint-disable-next-line no-unused-vars
import { onMounted, ref, watch } from "vue";
import { useRoute } from "vue-router";
import * as PIXI from 'pixi.js';
import {Spine} from 'pixi-spine';

const viewPortWidth=3164.0;
const viewPortHeight = viewPortWidth * window.screen.height / window.screen.width;

const app = new PIXI.Application({width:viewPortWidth,height:viewPortHeight});
const canvas = ref(null);

onMounted(()=>{
    console.log(canvas)
    window.ca=canvas
    canvas.value.appendChild(app.view);
    app.view.style="width:100%; height:100%;"

    let spineLoaderOptions = { metadata: { spineSkeletonScale: 1.0 } };
    app.loader
    .add('spineCharacter', '/spine/Azusa_swimsuit_home.skel', spineLoaderOptions)
    .load(function (loader, resources) {
        const animation = new Spine(resources.spineCharacter.spineData);

        // add the animation to the scene and render...
        app.stage.addChild(animation);
        animation.position.set(viewPortWidth/2,viewPortHeight);
        
        animation.state.addAnimation(0, 'Start_Idle_01', false);
        // animation.state.addAnimation(0,"Idle_01",true);
        animation.state.addListener(()=>{

        })
        
        app.start();
    });
})


</script>

<template>
  <div ref="canvas"></div>
</template>

<style scoped lang="scss"></style>
