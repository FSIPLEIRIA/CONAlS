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
	  stageLayers: [],
    }
  },
  
  methods: {
      async fetchImage() {
		//delay three seconds
		await new Promise(resolve => setTimeout(resolve, 3000));

        return Promise.resolve('./img/logo.jpg');
      
      },
	  loadingScreenLogic(stage){
		const rectangleSettings = {
			x: window.innerWidth/2,
			y: window.innerHeight/2,
			width: 100,
			height: 100,
			fill: 'red',
			rotation: 0,
			offset: {
				x: -50,
				y: -50
			}
		};
		var layer = new Konva.Layer();
		//draw 3 rectangles that rotate in the middle of the screen
		var rect1 = new Konva.Rect(rectangleSettings);
		let rect2sets = rectangleSettings;
		rect2sets.fill = 'green';
		rect2sets.offset.x = 50;
		rect2sets.offset.y = 50;
		var rect2 = new Konva.Rect(rect2sets);

		rect2sets.fill = 'blue';
		rect2sets.offset.x = 150;
		rect2sets.offset.y = 150;
		var rect3 = new Konva.Rect(rect2sets);
		layer.add(rect1);
		layer.add(rect2);
		layer.add(rect3);
		stage.add(layer);
		// make an animation
		let anim = new Konva.Animation((frame)=>{
			let time = frame.time,
			timeDif = frame.timeDiff, 
			frameRate = frame.frameRate;
			//make the first square move 100 y down 
			rect1.y(rect1.y() + 100 * (timeDif / 1000));
			//make the second square move 100 x right
			rect2.x(rect2.x() + 100 * (timeDif / 1000));
			//make the third square move 100 y up
			rect3.y(rect3.y() - 100 * (timeDif / 1000));
			//make the fourth square move 100 x left
			rect3.x(rect3.x() - 100 * (timeDif / 1000));
			//make the squares rotate 90 degrees
			rect1.rotation(rect1.rotation() + 90 * (timeDif / 1000));
			rect2.rotation(rect2.rotation() + 90 * (timeDif / 1000));
			rect3.rotation(rect3.rotation() + 90 * (timeDif / 1000));
			//if the squares go off the screen reset them to the center
			if(rect1.x() > window.innerWidth || rect1.x() < 0 || rect1.y() > window.innerHeight || rect1.y() < 0){
				rect1.x(window.innerWidth / 2);
				rect1.y(window.innerHeight / 2);
			}
			if(rect2.x() > window.innerWidth || rect2.x() < 0 || rect2.y() > window.innerHeight || rect2.y() < 0){
				rect2.x(window.innerWidth / 2);
				rect2.y(window.innerHeight / 2);
			}
			if(rect3.x() > window.innerWidth || rect3.x() < 0 || rect3.y() > window.innerHeight || rect3.y() < 0){
				rect3.x(window.innerWidth / 2);
				rect3.y(window.innerHeight / 2);
			}
		}, layer);
		anim.start();

	  }
  },
  mounted() {
    var stage = new Konva.Stage({
        container: 'container',
        width: window.innerWidth,
        height: window.innerHeight,
      });
    this.loadingScreenLogic(stage);

    this.fetchImage().then((image) => {
      stage.destroyChildren();

      var layer2 = new Konva.Layer();
      var layer3 = new Konva.Layer();
      var layer4 = new Konva.Layer();
      var layer5 = new Konva.Layer();
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
      stage.add(layer3);
      stage.add(layer4);
      stage.add(layer5);
      //onclick image draw a point
      //on left click add a point in image 
      //on right click add point 2 in image
      img.on('click', function(e) {
        //only left click
        if(e.evt.button != 0) return;

        layer3.destroyChildren();
        var mousePos = stage.getPointerPosition();
        var point = new Konva.Circle({
          x: mousePos.x,
          y: mousePos.y,
          radius: 5,
          fill: 'red',
         
          
        });
        layer3.add(point);
        layer3.draw();
      });
      img.on('contextmenu', function(e) {
        e.evt.preventDefault();
        //override right click
       
        var mousePos = stage.getPointerPosition();
        layer4.destroyChildren();
        var point = new Konva.Circle({
          x: mousePos.x,
          y: mousePos.y,
          radius: 5,
		  fill: 'blue'
          
        });
        layer4.add(point);
        layer4.draw();
      });
      //on enter draw a rectangle between the two points
      // on return key press draw rectangle
      
        
      window.addEventListener('keydown', function(e) {
        //only return key
        if(e.key != "Enter") return;
        var mousePos = stage.getPointerPosition();
        var point1 = layer3.getChildren()[0];
        var point2 = layer4.getChildren()[0];
        if(point1 == undefined || point2 == undefined) return;
        
        var rect = new Konva.Rect({
          x: point1.x(),
          y: point1.y(),
          width: point2.x() - point1.x(),
          height: point2.y() - point1.y(),
          stroke: 'red',

          
        });
        //this.drawnRetangles.push(rect)
        layer5.add(rect);
        layer5.draw();
      });

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
