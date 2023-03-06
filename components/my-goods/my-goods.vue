<template>
	<view class="goods-item">
		<!-- zuocede1左左左 -->
		<view class="goods-item-left">
			<image :src="goods.goods_small_logo || defaultPic" mode="" class="goods_pic"></image>
		</view>
		<!-- 右右右 -->
		<view class="goods-item-right">
			<!-- 商品的名字 -->
			<view class="goods-name">
				{{goods.goods_name}}
			</view>
			<!-- 商品的价格 -->
			<view class="goods-info-box">
				<view class="goods-price">
					￥{{goods.goods_price | tofixeds}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"my-goods",
		props:{
			goods:{
				type: Object,
				default:{}
			}
		},
		data() {
			
			return {
				//默认图片
				defaultPic:'https://i2.hdslb.com/bfs/archive/120623ef29707951857ff8e274e67a672ce71cad.jpg@672w_378h_1c_!web-home-common-cover.avif'
			};
		},
		filters: {
			tofixeds(num) {
				return Number(num).toFixed(2)
			}
		}
	}
</script>

<style lang="scss">
	.goods-item{
		display: flex;
		padding: 10px 5px;
		border-bottom: 1px solid #fofofo;
		.goods-item-left{
			margin-right: 5px;
			.goods_pic{
				width: 100px;
				height: 100px;
				display: block;
			}
		}
		.goods-item-right{
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			.goods-name{
				font-size: 13px;
			}
			.goods-info-box{
				.goods-price{
					color: #C00000;
					font-size: 16px;
				}
			}
		}
	}
</style>