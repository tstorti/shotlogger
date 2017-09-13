<template>
	<div>
		<div >
			<canvas v-on:click="getPosition" class="court" id="canvas" width="624" height="400"></canvas>
		</div>
		<div>
			<input  type="radio" name="outcome" value="made" v-model="outcomeToggle">
			<label for="genderChoice1">Made</label>
			<input type="radio" name="outcome" value="missed" v-model="outcomeToggle">
			<label for="genderChoice2">Missed</label>
		</div>
		<div>
			
		</div>
		<div class="last">
			<div>Last Added</div>
			<div>Shooter: {{lastShot.shooter}}</div>
			<div>Position: {{lastShot.x}}, {{lastShot.y}}</div>
			<div>Outcome: {{lastShot.outcome}}</div>
		</div>
	</div>
</template>

<script>


export default {	
	name: 'ShotLogger',
	data () {
		return {
			//TODO - have user configure the player set to be used for the session
			shooters:["player1", "player2", "player3", "player4"],
			missedShots:[],
			madeShots:[],
			lastShot:{
				"shooter":"",
				"outcome":"",
				"x":"",
				"y":""
			},
			outcomeToggle:"made",
		}
	},
	methods: {
		//get the position of the mouseclick
		getPosition: function(event){
			var rect = canvas.getBoundingClientRect();
			var x = event.clientX - rect.left; // x == the location of the click in the document - the location (relative to the left) of the canvas in the document
			var y = event.clientY - rect.top; // y == the location of the click in the document - the location (relative to the top) of the canvas in the document
			
			// This method will handle the coordinates and will draw them in the canvas.
			this.drawCoordinates(x,y);
		},

		drawCoordinates: function(x,y){
			var pointSize = 8; // Change according to the size of the point.
			var ctx = document.getElementById("canvas").getContext("2d");
			this.lastShot.x = x;
			this.lastShot.y = y;

			if(this.outcomeToggle ==="missed"){
				ctx.fillStyle = "#ff2626"; // Red color	
				this.lastShot.outcome = "Missed";
				this.missedShots.push(this.lastShot);
			}
			else{
				ctx.fillStyle = "#0000FF"; // Blue color	
				this.lastShot.outcome = "Made";
				this.madeShots.push(this.lastShot);
			}
			
			ctx.beginPath(); //Start path
			ctx.arc(x, y, pointSize, 0, Math.PI * 2, true); // Draw a circle point using the arc function of the canvas with a point structure.
			ctx.fill(); // Close the path and fill.

			this.lastX = x;
			this.lastY = y;
			
		},
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.court{
		/* background-color: gray; */
		background:url("../assets/img/court.svg")
	}
	.last{
		margin:25px;
	}
</style>
