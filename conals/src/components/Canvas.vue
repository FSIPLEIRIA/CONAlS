<template lang="pug">
.screen
  .navbar
    .navbar-logo
      img(src="../../public/favicon.png")
    .navbar-title
      h1 Conals
  #container
</template>
<script>
//
import { Stage, Layer, Image } from 'vue-konva'
import Konva from 'konva'
import fs from 'fs'
const rectangleSettings = {
        x: window.innerWidth/2,
        y: window.innerHeight/2,
        width: 100,
        height: 100,
        fill: 'red',
        stroke: 'black',
        strokeWidth: 4,
        draggable: true,
        rotation: 0,
        offset: {
            x: -50,
            y: -50
        }
    };
export default {
  name: 'Canvas',
  components: {
	Stage,
	Layer,
	Image
  },
  data () {
    return {
      loaded: false,
      stageOptions: {
      width: window.innerWidth,
      height: window.innerHeight
      
      }, 
      

    }
  },
  
  methods: {
      async fetchImage() {
        return Promise.resolve('./img/logo.jpg');
      
      }
  },
  mounted() {
    var stage = new Konva.Stage({
        container: 'container',
        width: window.innerWidth,
        height: window.innerHeight,
      });
    var layer = new Konva.Layer();
    //draw 3 rectangles that rotate in the middle of the screen
    var rect1 = new Konva.Rect(rectangleSettings);
    let rect2sets = rectangleSettings;
    rect2sets.offset.x = 50;
    rect2sets.offset.y = 50;
    var rect2 = new Konva.Rect(rect2sets);
    rect2sets.offset.x = 150;
    rect2sets.offset.y = 150;
    var rect3 = new Konva.Rect(rect2sets);
    layer.add(rect1);
    layer.add(rect2);
    layer.add(rect3);
    stage.add(layer);

    this.fetchImage().then((image) => {
      stage.destroyChildren();

      var layer2 = new Konva.Layer();
      //centered image
      var imageObj = new window.Image();
      
      var img = new Konva.Image({
        x: (window.innerWidth)/2,
        y: (window.innerHeight)/2,
        width: 500,
        height: 500,
        image: imageObj,
        draggable: true, 
        
        
      });
      imageObj.src = image;
      layer2.add(img);
      stage.add(layer2);

      //onclick image draw a point


      //stage.batchDraw();

      
    }).catch((err) => {
      console.log(err);
    });

    //get me an image


  },

}
</script>

<style scoped>
.navbar {
  position: fixed;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  
  width: 100%;
  height: 60px;
  background-color: #1B3147;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5em;
  font-weight: bold;
}


</style>
