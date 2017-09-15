<template>
	<div>
		<div>Shooting Accuracy</div>
		<!-- <div >
			<canvas  class="court" id="canvas" width="624" height="400"></canvas>
		</div> -->
		<svg id="playerchart" width="624" height="400">test container</svg>
		<div id="shot-chart"></div>
	</div>
	
</template>

<script>

var d3 = require("d3");
//TODO - this is still throwing an error when adding to the d3 object.
var hexbin = require("d3-hexbin");
var d3Tip = require("d3-tip");
window.d3 = d3;
var d3ShotChart = require("d3-shotchart");

d3 = Object.assign({}, d3, hexbin, d3Tip, d3ShotChart);


console.log(d3);

//console.log(d3);

export default {	
	name: 'ShotChart',
	data () {
		return {
			//TODO - have user configure the player set to be used for the session
			shooters:["player1", "player2", "player3", "player4"],
			basketLocation: {
				"x":312,
				"y":378
			},
			playerShots:[
				{
					"shooter":"player1",
					"outcome":"made",
					"x":311.5,
					"y":62.28
				},
				{
					"shooter":"player1",
					"outcome":"made",
					"x":311.5,
					"y":62.28
				},
				{
					"shooter":"player1",
					"outcome":"missed",
					"x":311.5,
					"y":62.28
				},
				{
					"shooter":"player1",
					"outcome":"made",
					"x":280.5,
					"y":62.28
				},
				{
					"shooter":"player1",
					"outcome":"made",
					"x":406.5,
					"y":114.28
				},
				{
					"shooter":"player1",
					"outcome":"made",
					"x":246.5,
					"y":213.28
				},
				{
					"shooter":"player1",
					"outcome":"missed",
					"x":246.5,
					"y":213.28
				},

			],
			leagueShots:[
				{
					"shooter":"player1",
					"outcome":"made",
					"x":"",
					"y":""
				}
			]
		}
	},
	mounted() {
		//test to make sure hexbin library is working with d3
		this.hexbinTest();
		//this.playerShotDistr();
    },
	methods: {
		//get the position of the mouseclick
		hexbinTest: function(){
			var target = document.getElementById("playerchart");
			
			var svg = d3.select(target),
				margin = {top: 20, right: 20, bottom: 30, left: 40},
				width = +svg.attr("width") - margin.left - margin.right,
				height = +svg.attr("height") - margin.top - margin.bottom,
				g = svg.append("g").attr("transform", "translate(" + margin.left + "," + margin.top + ")");

			var randomX = d3.randomNormal(width / 2, 80),
				randomY = d3.randomNormal(height / 2, 80),
				points = d3.range(2000).map(function() { return [randomX(), randomY()]; });

			var radius = d3.scaleSqrt()
				.domain([0, 50])
				.range([0, 20]);

			var hexbin = d3.hexbin()
				.radius(20)
				.extent([[0, 0], [width, height]]);

			var x = d3.scaleLinear()
				.domain([0, width])
				.range([0, width]);

			var y = d3.scaleLinear()
				.domain([0, height])
				.range([height, 0]);

			g.append("clipPath")
				.attr("id", "clip")
			.append("rect")
				.attr("width", width)
				.attr("height", height);

			g.append("g")
				.attr("class", "hexagon")
				.attr("clip-path", "url(#clip)")
			.selectAll("path")
			.data(hexbin(points))
			.enter().append("path")
				.attr("d", function(d) { return hexbin.hexagon(radius(d.length)); })
				.attr("transform", function(d) { return "translate(" + d.x + "," + d.y + ")"; });
		},
		playerShotDistr: function(){
			var courtSelection = d3.select("#shot-chart");
    		var court = d3.court().width(600);
    		var shots = d3.shots().shotRenderThreshold(1).displayToolTips(true).displayType("hexbin");
    		courtSelection.call(court);
    		courtSelection.datum(data).call(shots);
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

	.hexagon {
		fill: steelblue;
		stroke: #fff;
		stroke-width: 0.5px;
	}
</style>
