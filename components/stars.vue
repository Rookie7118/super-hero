<template name="starts">
	<view>
		<view class="movie-score-wrapper">
			<!-- 			<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
			<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
			<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
			<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
			<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
			<image src="../../static/icos/star-gray.png" class="star-icon"></image>
			<image src="../../static/icos/star-gray.png" class="star-icon"></image>
			<view class="movie-score" v-if="showScore">
				{{innerScore}}
			</view> -->
			<image 
			v-for="yellow in yellowStar" 
			src="../../static/icos/star-yellow.png" 
			class="star-icon"></image>
			<image 
			v-for="gray in grayStar"
			src="../../static/icos/star-gray.png" 
			class="star-icon"></image>
			<view class="movie-score" v-if="visible">
				{{innerScore}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "stars",
		data() {
			return {
				yellowStar: 0,
				grayStar: 5
			};
		},
		props: {
			innerScore: 0,
			visible: false
		},
		created() {
			var tempScore = 0;
			if (this.innerScore != null && this.innerScore != undefined && this.innerScore != '') {
				tempScore = this.innerScore;
			}
			var yellowStar = parseInt(tempScore / 2);
			var grayStar = 5 - yellowStar;

			this.yellowStar = yellowStar;
			this.grayStar = grayStar;
			
		}
		// created: () => {
		// 	var tempScore = 0;
		// 	if (this.innerScore != null && this.innerScore != undefined && this.innerScore != '') {
		// 		tempScore = this.innerScore;
		// 	}
		// 	var yellowStar = parseInt(tempScore / 2);
		// 	var grayStar = 5 - yellowStar;
		// 	
		// 	this.yellowStar = yellowStar;
		// 	this.grayStar = grayStar;
		// }
	}
</script>

<style>
	.movie-score-wrapper {
		display: flex;
		flex-direction: row;
		width: 200upx;
	}

	.star-icon {
		width: 20upx;
		height: 20upx;
		margin-top: 6upx;
	}

	.movie-score {
		font-size: 12px;
		color: grey;
		margin-left: 8upx;
	}
</style>
