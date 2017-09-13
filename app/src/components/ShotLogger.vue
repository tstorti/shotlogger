<template>
	<div>
		<canvas v-on:click="getPosition" class="court" id="canvas" width="600" height="600"></canvas>
	</div>
</template>

<script>


export default {	
	name: 'ShotLogger',
	data () {
		return {
			missedShots:[],
			madeShots:[],
		}
	},
	methods: {

		// Event will be a click event which can be retrieved as first parameter in the addEventListener(function(event){}); or in jQuery with $("selector").click(function(event){});
		getPosition: function(event){
			var rect = canvas.getBoundingClientRect();
			var x = event.clientX - rect.left; // x == the location of the click in the document - the location (relative to the left) of the canvas in the document
			var y = event.clientY - rect.top; // y == the location of the click in the document - the location (relative to the top) of the canvas in the document
			
			// This method will handle the coordinates and will draw them in the canvas.
			this.drawCoordinates(x,y);
		},

		drawCoordinates: function(x,y){
			var pointSize = 3; // Change according to the size of the point.
			var ctx = document.getElementById("canvas").getContext("2d");

			var pointSize = 3;

			ctx.fillStyle = "#ff2626"; // Red color

			ctx.beginPath(); //Start path
			ctx.arc(x, y, pointSize, 0, Math.PI * 2, true); // Draw a point using the arc function of the canvas with a point structure.
			ctx.fill(); // Close the path and fill.
		}
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.court{
		background-color: gray;
	}
</style>
