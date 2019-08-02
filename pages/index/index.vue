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
					
					<stars :innerScore="poster.score" showScore="1" ></stars>

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
		
		
		<!-- 热门预告 start -->
		<view class="page-block super-hot">
			<view class="hot-title-wrapper">
				<image src="../../static/icos/interest.png" mode="" class="hot-icon"></image>
				<view class="hot-title">
					热门预告
				</view>
			</view>
		</view>
		<view class="page-block hot-trailers">
			<video
			v-for="trailer in hotTrailerList "
			class="hot-movie-single"
			:poster = "trailer.poster"
			:src="trailer.trailer"></video>
		</view>
		
		<!-- 热门预告 end -->
		
		<!-- 猜你喜欢 start -->
		<view class="page-block super-hot">
			<view class="hot-title-wrapper">
				<image src="../../static/icos/guess-u-like.png" mode="" class="hot-icon"></image>
				<view class="hot-title">
					猜你喜欢
				</view>
			</view>
		</view>
		<view class="page-block guess-u-like">
			<view class="single-like-movie" v-for="(movie, index) in guessYouLikeList">
				<image :src="movie.cover" class="like-movie-poster"></image>
				<view class="movie-desc">
					<view class="movie-title">
						{{movie.name}}
					</view>
					<stars :innerScore="movie.score" showScore="1" ></stars>
					<view class="basic-info">
						{{movie.basicInfo}}
					</view>
					<view class="release-date">
						{{movie.releaseDate}}
					</view>
				</view>
				<view class="movie-oper" :data-index="index" @click="praiseMe">
					<image src="../../static/icos/praise.png" class="praise-icon"></image>
					<view class="praise-me">
						赞一下
					</view>
					<view class="praise-me animation-opaticty" :animation="animationDataArr[index]">
						+1
					</view>
				</view>
			</view>
			
		</view>
		<!-- 猜你喜欢 end -->

	</view>
</template>

<script>
	import localCarouselList from "../../static/json/carousel_list.json"
	import hotSuperheroList from "../../static/json/hot_super_hero.json"
	import hotTrailerList from "../../static/json/hot_trailer.json"
	import guessYouLikeList from "../../static/json/guess_you_like.json"
	
	
	import stars from "../../components/stars.vue"
	// console.log(hotSuperheroList)
	// debugger
	// import common from "../../common/common.js"
	export default {
		data() {
			return {
				title: 'Hello',
				carouselList: [],
				hotSuperheroList: [],
				hotTrailerList: [],
				guessYouLikeList:[],
				animationData:{},
				animationDataArr:[{},{},{},{},{}]
			}
		},
		onLoad() {
			
			// #ifdef APP-PLUS || MP-WEIXIN
			this.animation = uni.createAnimation({
			})
			// #endif
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
			
			uni.request({
				url: this.serverUrl + 'index/movie/hot?type=trailer',
				method: 'POST',
				// success: res => {
				// 	res.data = hotSuperheroList
				// 	this.hotSuperheroList = res.data.data
				// 	console.log(this.hotSuperheroList)
				// },
				fail: () => {
				},
				complete: () => {
					this.hotTrailerList = hotTrailerList.data
					this.guessYouLikeList = guessYouLikeList.data;
					console.log(this.hotTrailerList);
				}
			});

		},
		onUnload: () => {
			this.animationData = {};
			this.animationDataArr = [{},{},{},{},{}];
		},
		onPullDownRefresh() {
			this.refresh()
			// this.hotTrailerList.reverse()
		},
		methods: {
			// 
			refresh() {
				uni.showLoading({
					mask:true
				})
				uni.showNavigationBarLoading()
				this.carouselList.reverse()
				this.guessYouLikeList.reverse()
				setTimeout(function() {
					uni.stopPullDownRefresh()
					uni.hideLoading()
					uni.hideNavigationBarLoading()
				}, 2000);
			},
			
			praiseMe(e) {
				// #ifdef APP-PLUS || MP-WEIXIN
				
				console.log("praise me")
				var index = e.currentTarget.dataset.index
				console.log(index)
				
				this.animation.translateY(-60).opacity(1).step({
					duration:400
				})
				this.animationData = this.animation
				this.animationDataArr[index] = this.animationData.export()
				console.log(this.animationDataArr)
				
				setTimeout(function() {
					this.animation.translateY(0).opacity(0).step({
						duration:0
					})
					this.animationData = this.animation
					this.animationDataArr[index] = this.animationData.export()
				}.bind(this), 500);
				
				// #endif
			}
			
		
		},
		components:{
			stars
		}
	}
</script>

<style>
	@import url("index.css");
</style>
