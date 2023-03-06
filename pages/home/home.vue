<template>
	<view>
		<!-- 搜索区域 -->
		<view class="search-box">
			<my-search @click='gotosearch'></my-search>
		</view>
		<!-- 轮播图区域 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="(item,i) in swiperlist" :key="i">
				<navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
					<image :src="item.image_src" ></image>
				</navigator>
			</swiper-item>
			
		</swiper>
		<!-- 分类导航区 -->
		<view class="nav_list">
			<view v-for="(item,index) in navList" :key="index" @click="navClickHandler(item)">
				<image :src="item.image_src" mode="" class="nav-img"></image>
			</view>
		</view>
		<!-- 楼层区域 -->
		<view class="floor-list">
			<!-- 楼层的item项 -->
			<view class="floor-item" v-for="(item,i) in floorList" :key="i" >
				<!-- 楼层的标题 -->
				<image :src="item.floor_title.image_src" mode="" class="floor-title"></image>
				<!-- 楼层的图片 -->
				<view class="floor-img-box">
					<!-- 左 -->
					<navigator class="left-img-box" :url="item.product_list[0].url">
						<image :src="item.product_list[0].image_src" :style="{width:item.product_list[0].image_width+'rpx'}" mode="widthFix"></image>
						
					</navigator>
					<!-- 右 -->
					<view class="right-img-box">
						<navigator v-for="(item2,index) in item.product_list" :key="index" class="right-img-item" v-if="index !== 0" :url="item2.url">
							<image :src="item2.image_src" mode="widthFix" :style="{width:item2.image_width + 'rpx'}"></image>
						</navigator>
					</view>
					
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//轮播图数据列表
				swiperlist:[],
				navList:[],
				floorList:[]
			};
		},
		onLoad() {
			this.getSwiperList(),
			this.getNavList(),
			this.getFloorList()
		},
		methods:{
			async getSwiperList(){
				const {data :res}= await uni.$http.get('https://api-hmugo-web.itheima.net/api/public/v1/home/swiperdata')
				//console.log(res)
				if(res.meta.status !== 200){
					return uni.$showMsg()
				}
				this.swiperlist = res.message
				uni.$showMsg('数据请求成功')
			 },
				 
			 async getNavList(){
				 const {data :res}= await uni.$http.get('https://api-hmugo-web.itheima.net/api/public/v1/home/catitems')
				 //console.log(res)
				 if(res.meta.status !== 200){
				 	return uni.$showMsg()
				 }
				 this.navList = res.message
				 uni.$showMsg('数据请求成功')
			 },
			 async getFloorList(){
			 				 const {data :res}= await uni.$http.get('https://api-hmugo-web.itheima.net/api/public/v1/home/floordata')
			 				 //console.log(res)
			 				 if(res.meta.status !== 200){
			 				 	return uni.$showMsg()
			 				 }
							 //对数据进行处理
							 res.message.forEach(floor=>{
								 floor.product_list.forEach(prod=>{
									 prod.url='/subpkg/goods_list/goods_list?'+prod.navigator_url.split('?')[1]
								 })
							 })
								 
							 
			 				 this.floorList = res.message
			 				 uni.$showMsg('数据请求成功')
			 },
			 navClickHandler(item){
				 //console.log(item)
				 if(item.name === '分类')
				 {
					 // 切换到tabbar 下面的导航栏，要用switchTab
					 uni.switchTab({
					 	url:'/pages/cate/cate'
					 })
				 }
			 },
			 gotosearch(){
				 uni.navigateTo({
				 	url: '/subpkg/search/search'
				 })
			 }
		}
	}
</script>

<style lang="scss">
	swiper{
		height: 330rpx;
		.swiper-item,
		image {
			width: 100%;
			height: 100%;
		}
	}
	.nav_list{
		display: flex;
		justify-content: space-around;
		margin: 15px 0;
		.nav-img{
			width: 128rpx;
			height: 140rpx;
		}
	}
	.floor-title {
		width: 100%;
		height: 60rpx;
	}
	.right-img-box {
		display: flex;
		//必要时换行
		flex-wrap: wrap;
		justify-content: space-around;
	}
	.floor-img-box {
		display: flex;
		padding-left: 10rpx;
		
	}
	.search-box {
		position: sticky;
		top: 0;
		z-index: 999;
	}
</style>
