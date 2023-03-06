<template>
	<view>
		<view class="goods-list">
			<view v-for="(goods,i) in goodsList" :key="i" @click="gotoDetail(goods)">
				<my-goods :goods="goods"></my-goods>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//请求参数对象
				queryObj:{
					query:'',
					cid:'',
					pagenum:1,
					pagesize:20,
					
				},
				goodsList:[],
				total: 0,
				isloading: false
			};
		},
		onLoad(options) {
			// console.log(options)
			this.queryObj.query = options.query || ''
			this.queryObj.cid = options.cid || ''
			this.getGoodsList()
		},
		methods:{
			async getGoodsList(cd){
				this.isloading = true
				const {'data':res} = await uni.$http.get('https://api-hmugo-web.itheima.net/api/public/v1/goods/search',this.queryObj)
				this.isloading = false
				cd && cd()
				if(res.meta.status !== 200){
				 	return uni.$showMsg()
				 }
				 
				this.goodsList = [...this.goodsList,...res.message.goods]
				this.total = res.message.total
				
				 
				 
			},
			gotoDetail(goods){
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?goods_id=' + goods.goods_id
				})
			}
		},
		onReachBottom(){
			if(this.isloading) return
			this.queryObj.pagenum++
			this.getGoodsList()
		},
		onPullDownRefresh(){
			this.queryObj.pagenum = 1
			this.total = 0
			this.isloading = false
			this.goodsList= []
			
			this.getGoodsList(()=>{
				//关闭下拉刷新
				uni.stopPullDownRefresh()
			})
		}
		
	}
</script>

<style lang="scss">
	
</style>
