<template>
	<div>
		<div>Shooting Accuracy</div>
		<div id="chart1"></div>
	</div>	
</template>

<script>

var sample = require("../assets/js/data.js");
export default {	
	name: 'ShotChart',
	data () {
		return {
			//TODO - have user configure the player set to be used for the session
			shooters:["player1", "player2", "player3", "player4"],
			data:sample
		}
	},
	mounted() {
		this.playerShotDistr();
    },
	methods: {
		playerShotDistr: function(){
			console.log(this.data);
			var heatRange = ['#5458A2', '#6689BB', '#FADC97', '#F08460', '#B02B48'];
			
			d3.select(document.getElementById('chart1'))
			.append("svg")
				.chart("BasketballShotChart", {
				width: 600, 
				title: 'Sample Data',
				hexagonFillValue: function(d) {  return d.z; }, 
				// reverse the heat range to map our z values to other colors
				heatScale: d3.scale.quantile()
					.domain([-2.5, 2.5])
					.range(['#5458A2', '#6689BB', '#FADC97', '#F08460', '#B02B48']),
				hexagonBin: function (point, bin) {
					var currentZ = bin.z || 0;
					var totalAttempts = bin.attempts || 0;
					var totalZ = currentZ * totalAttempts;

					var attempts = point.attempts || 1;
					bin.attempts = totalAttempts + attempts;
					bin.z = (totalZ + (point.z * attempts))/bin.attempts;
				},
				// update radius threshold to at least 4 shots to clean up the chart
				hexagonRadiusThreshold: 4,
				})
			.draw(this.data); 
		},
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
