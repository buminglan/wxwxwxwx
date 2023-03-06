<template>
	<view>
		<view class="search-box">
			<uni-search-bar  @input="input" cancelButton="none"></uni-search-bar>
			<!-- @confirm="search" -->
		</view>
		
		<!-- 搜索结果 -->
		<view class="sugg-list" v-if="searchResults.length !== 0">
			<view class="sugg-item" v-for="(item,i) in searchResults" :key="i" @click="gotoDetail(item)">
				<view class="goods-name">
					{{item.goods_name }}
					
				</view>
				<uni-icons type="arrowright" size="16" "></uni-icons>
			</view>
			
		</view>
		<!-- 搜索历史 -->
		<view class="history-box" v-else>
			<!-- 标题区域 -->
			<view class="history-title" @click="clean">
				<text>搜索历史</text>
				<uni-icons type="trash" size="17" ></uni-icons>
			</view>
			<!-- liebqvyu -->
			<view class="history-list">
				<uni-tag :text="item" v-for="(item,i) in histories" :key="i" @click="gotoGoodsList(item)"></uni-tag>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				timer:null,
				kw:'',
				//搜索的结果列表
				searchResults:[],
				historyList:[]
			};
		},
		onLoad(){
			this.historyList = JSON.parse(uni.getStorageSync('kw' || []))
		},
		methods:{
			input(e){
				// console.log(e);
				clearTimeout(this.timer)
				setTimeout(()=>{
					this.kw=e
					this.getSearchList()
				},500)
			},
			async getSearchList(){
				if(this.kw.length === 0)
				{
					this.searchResults = []
					return
				}
				 const {data:res}=await uni.$http.get('https://api-hmugo-web.itheima.net/api/public/v1/goods/qsearch' ,{query:this.kw})
				 if(res.meta.status !== 200){
				 	return uni.$showMsg()
				 }
				 
				this.searchResults = res.message
				 
				 uni.$showMsg('数据请求成功')
				 this.historyList.push(this.kw)
				 this.saveSearchHistory()
			}, 
			gotoDetail(item){
				// console.log(item.goods_id)
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?goods_id=' +item.goods_id
				})
			},
			//保存历史搜索记录
			saveSearchHistory(){
				//this.historyList.push(this.kw)
				const  set = new Set(this.historyList)
				set.delete(this.kw)
				set.add(this.kw)
				this.historyList = Array.from(set)
				
				uni.setStorageSync('kw',JSON.stringify(this.historyList))
				
			},
			clean(){
				console.log(666)
				this.historyList = []
				uni.setStorageSync('kw','[]')
				
			},
			gotoGoodsList(kw){
				uni.navigateTo({
					url:'/subpkg/goods_list/goods_list?query=' + kw
				})
			}
		},
		computed:{
			histories(){
				return [...this.historyList].reverse()
			}
		}
	}
</script>

<style lang="scss">
	.search-box {
		position: sticky;
		top: 0;
		z-index: 999;
	}
	.sugg-list {
		padding: 0 5px;
		.sugg-item {
			display: flex;
			align-items: center;
			justify-content: space-between;
			font-size: 12px;
			padding: 13px 0;
			border: 1px solid #efefef;
			.goods-name {
				white-space: nowrap;
				overflow: hidden;
				text-overflow: ellipsis;
			}
		}
	}
	.history-box {
		padding: 0 5px;
		.history-title {
			display: flex;
			justify-content: space-between;
			
		}
	}

</style>
