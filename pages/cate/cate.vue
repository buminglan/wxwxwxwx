<template>
	<view>
		<my-search @click='gotoSearch'></my-search>
		<view class="scoll-view-container">
			<!-- 左 -->
			<scroll-view scroll-y="true" :style="{height: wh + 'px'}" class="left-scroll-view">
				<block v-for="(item,i) in cateList" :key="i">
					<view :class="['left-scroll-view-item',i === active ? 'active':'']" @click="activeChange(i)">{{item.cat_name}}</view>
				</block>
				
			</scroll-view>
			<!-- 右 -->
			<scroll-view scroll-y="true" :style="{height: wh + 'px'}" :scroll-top="scrollTop">
				<view class="cate-lv2" v-for="(item2,i2) in cateLevel2" :key="i2">
					<!-- 二级分类的标题 -->
					<view class="cate-lv2-title">
						/ {{item2.cat_name}} /
					</view>
					<!-- 二级分类下的三级分类列表 -->
					<view class="cate-lv3-list">
						<view class="cate-lv3-ltem" v-for="(item3,i3) in item2.children" :key="i3" @click="gotoGoodList(item3)">
							<image :src="item3.cat_icon" mode=""></image>
							<text>{{item3.cat_name}}</text>
						</view>
					</view>
				</view>
			</scroll-view>
		</view>

	</view>
</template>

<script>
	import badgeMix from '@/mixins/tabber-badge.js'
	export default {
		mixins:[badgeMix],
		data() {
			return {
				wh: 0,
				cateList:[],
				active: 0,
				cateLevel2:[],
				scrollTop:0
			};
		},
		onLoad(){
			
			const apple = uni.getSystemInfoSync()
			this.wh = apple.windowHeight - 50
			this.getCateList()
		},
		methods:{
			async getCateList(){
				
				const {data :res}= await uni.$http.get('https://api-hmugo-web.itheima.net/api/public/v1/categories')
				if(res.meta.status !== 200){
					return uni.$showMsg()
				}
				
				 this.cateList = res.message
				 this.cateLevel2 =res.message[0].children
				uni.$showMsg('数据请求成功')
			},
			activeChange(i){
				this.active=i
				
				//重新为二级分类赋值
				this.cateLevel2 = this.cateList[i].children
				
				this.scrollTop = this.scrollTop === 0 ? 1 : 0
				
			},
			gotoGoodList(item){
				uni.navigateTo({
					url:'/subpkg/goods_list/goods_list?cid=' + item.cat_id
				})
			},
			gotoSearch(){
				uni.navigateTo({
					url: '/subpkg/search/search'
				})
			}
		}
	}
</script>

<style lang="scss">
	.scoll-view-container {
		display: flex;
	}
	.left-scroll-view {
		width: 120px;
	}
	.left-scroll-view-item {
		background-color: #F7F7F7;
		line-height: 60px;
		text-align: center;
		font-size: 12px;
		&.active {
			background-color: #fffff;
			position: relative;
			&::before {
				content: '';
				display: block;
				width: 3px;  
				height: 30px;
				background-color: #C00000;
				position: absolute;
				top: 50%;
				left: 0;
				transform: translateY(-50%);
			}
		}
	}
	
	
	.cate-lv2-title {
		font-size: 12px;
		text-align: center;
		padding: 15px 0;
	}
	.cate-lv3-list {
		display: flex;
		flex-wrap: wrap;
		.cate-lv3-ltem {
			width: 33.33%;
			display: flex;
			flex-direction: column;
			justify-content: center;//纵向居中
			align-items: center;//横向居中
			margin-bottom: 10px;
			image {
				width: 60px;
				height: 60px;
			}
			text {
				font-size: 12px;
			}
		}
	}
</style>
