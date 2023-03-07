<template>
	<view class="goods-item">
		<!-- zuocede1左左左 -->
		<view class="goods-item-left">
			 <radio :checked="goods.goods_state" color="#C00000" v-if="showRadio" @click="radioClickHandler"></radio>
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
				<!-- 商品数量 -->
				<uni-number-box :min="1" :value="goods.goods_count" v-if="showNum" @change="numChangeHandler"></uni-number-box>
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
			},
			showRadio: {
			      type: Boolean,
			      // 如果外界没有指定 show-radio 属性的值，则默认不展示 radio 组件
			      default: false,
			},
			showNum: {
			      type: Boolean,
			      default: false,
			},
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
		},
		methods: {
		  // radio 组件的点击事件处理函数
		  radioClickHandler() {
		    // 通过 this.$emit() 触发外界通过 @ 绑定的 radio-change 事件，
		    // 同时把商品的 Id 和 勾选状态 作为参数传递给 radio-change 事件处理函数
		    this.$emit('radio-change', {
		      // 商品的 Id
		      goods_id: this.goods.goods_id,
		      // 商品最新的勾选状态
		      goods_state: !this.goods.goods_state
		    })
		  },
		  numChangeHandler(val) {
		      // 通过 this.$emit() 触发外界通过 @ 绑定的 num-change 事件
		      this.$emit('num-change', {
		        // 商品的 Id
		        goods_id: this.goods.goods_id,
		        // 商品的最新数量
		        goods_count: +val
		      })
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
			display: flex;
			justify-content: space-between;
			align-items: center;
			.goods_pic{
				width: 100px;
				height: 100px;
				display: block;
			}
		}
		.goods-item-right{
			display: flex;
			flex: 1;
			flex-direction: column;
			justify-content: space-between;
			.goods-name{
				font-size: 13px;
			}
			.goods-info-box{
				display: flex;
				align-items: center;
				justify-content: space-between;
				.goods-price{
					color: #C00000;
					font-size: 16px;
				}
			}
		}
	}
</style>