<template lang="pug">
div
  v-stage(width="500" height="500" :config="stageOptions")  
    v-layer(v-if="!loaded")
      v-rect(x="0" y="0" width="100" height="100" fill="red" )
      v-rect(x="100" y="100" width="100" height="100" fill="blue" )
      v-rect(x="200" y="200" width="100" height="100" fill="green")
    v-layer(v-if="loaded")
      v-image(
		:image="image" 	  	
	  )
      

</template>
<script>
//
import { Stage, Layer, Image } from 'vue-konva'
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
	  image: null

	}
  }, 
  created() {

	const image = new window.Image();
	
    image.src = "./img/logo.jpg";
    image.onload = () => {
		this.loaded = true;
      // set image only when it is loaded
      this.image = image;
    
	};
  }
}
</script>

<style scoped>

</style>
