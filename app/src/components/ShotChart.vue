<template>
	<div>
		<div>Shooting Accuracy</div>
		<button v-on:click="playerShotDistr(gameData)">Show Distribution Data</button>
		<div id="chart1"></div>
	</div>	
</template>

<script>

//test data
var sample = require("../assets/js/data.js");

export default {	
	name: 'ShotChart',
	//grab data from shotLogger for display
	props:["newData"],
	data () {
		return {
			//TODO - have user configure the player to be used for the chart
			//shooters:["player1", "player2", "player3", "player4"],
			data:sample,
			gameData:[],
		}
	},

	watch: {
		//once data loads from stats component, display player data
        newData: function(){
			this.translateData();
        }
	},
	
	methods: {
		playerShotDistr: function(data){
			//clear previous heat chart
			d3.select("svg").remove();
			//draw new heatmap
			var heatRange = ['#5458A2', '#6689BB', '#FADC97', '#F08460', '#B02B48'];	
			d3.select(document.getElementById('chart1'))
			.append("svg")
				.chart("BasketballShotChart", {
				width: 600, 
				title: 'Sample Data',
				})
			.draw(data); 
		},
		
		calcDistance:function(){
			//court length (y coord) goes from 0 to 35
			//court width (x coord) goes from 0 to 50
		},
		translateData:function(){
			//convert input data from canvas into readable scale and x,y layout for d3
			this.gameData = [];
			for(var i=0; i<this.newData.length;i++){
				var newShot={};
				newShot.x = this.newData[i].x * (50/600); 
				newShot.y = (400 - this.newData[i].y) * (35/400); 
				newShot.z = this.newData[i].z;
				newShot.shooter = this.newData[i].shooter;
				newShot.made = this.newData[i].made;
				newShot.attempts = this.newData[i].attempts;
				this.gameData.push(newShot);
			}
		}

    }
}
</script>

<style>
	.shot-chart-court *{fill:transparent;stroke:#333;stroke-width:0.1};
	.shot-chart-court-ft-circle-bottom{stroke-dasharray:1.5, 1};
	.shot-chart-court-hoop,.shot-chart-court-backboard{z-index:100};
	.shot-chart-title{font-size:15%;text-transform:uppercase};
	.legend text{font-size:4%};
</style>
