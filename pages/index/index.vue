<template>
	<view class="page">
		<!-- 轮播图 start -->
		<swiper :indicator-dots="true" :autoplay="false" circular="true" class="carousel">
			<swiper-item v-for="carousel in carouselList">
				<image :src="carousel.image" class="carousel"></image>
			</swiper-item>
		</swiper>
		<!-- 轮播图 end -->

		<!-- 热门超英 start -->
		<view class="page-block super-hot">
			<view class="hot-title-wrapper">
				<image src="../../static/icos/hot.png" mode="" class="hot-icon"></image>
				<view class="hot-title">
					热门超英
				</view>
			</view>
		</view>
		<scroll-view scroll-x="true" class="page-block hot" >

			<view class="single-poster" v-for="poster in hotSuperheroList">
				<view class="poster-wrapper">
					<image :src="poster.cover" mode="" class="poster"></image>
					<view class="movie-name">
						{{poster.name}}
					</view>
					
					<stars :innerScore="poster.score" visible="false" ></stars>

<!-- 					<view class="movie-score-wrapper">
						<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
						<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
						<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
						<image src="../../static/icos/star-yellow.png" class="star-icon"></image>
						<image src="../../static/icos/star-gray.png" class="star-icon"></image>
						<view class="movie-score">
							{{poster.score}}
						</view>
					</view> -->
				</view>
			</view>

		</scroll-view>

		<!-- 热门超英 end -->

	</view>
</template>

<script>
	import localCarouselList from "../../static/json/carousel_list.json"
	import hotSuperheroList from "../../static/json/hot_super_hero.json"
	import stars from "../../components/stars.vue"
	// console.log(hotSuperheroList)
	// debugger
	// import common from "../../common/common.js"
	export default {
		data() {
			return {
				title: 'Hello',
				carouselList: [],
				hotSuperheroList: []
			}
		},
		onLoad() {
			// var serverUrl = common.serverUrl;
			uni.request({
				url: this.serverUrl + '/index/carousel/list',
				method: 'POST',
				success: res => {
					if (res.statusCode == 200) {
						res.data = localCarouselList;
						this.carouselList = res.data.data;
					}

				},
				fail: () => {},
				complete: () => {}
			});


			uni.request({
				url: this.serverUrl + 'index/movie/hot?type=superhero',
				method: 'POST',
				// success: res => {
				// 	res.data = hotSuperheroList
				// 	this.hotSuperheroList = res.data.data
				// 	console.log(this.hotSuperheroList)
				// },
				fail: () => {
				},
				complete: () => {
					this.hotSuperheroList = hotSuperheroList.data
				}
			});

		},
		methods: {

		},
		components:{
			stars
		}
	}
</script>

<style>
	@import url("index.css");
</style>
