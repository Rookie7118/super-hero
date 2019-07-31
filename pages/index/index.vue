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
			<view class="">
				<image src="../../static/icos/hot.png" mode=""></image>
				<view class="">
					超英热门
				</view>
			</view>
		</view>
		
		<!-- 热门超英 end -->
		
	</view>
</template>

<script>
	
	import localCarouselList from "../../static/json/carousel_list.json"
	// import common from "../../common/common.js"
	export default {
		data() {
			return {
				title: 'Hello',
				carouselList:[]
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

		},
		methods: {

		}
	}
</script>

<style>
	@import url("index.css");
</style>
