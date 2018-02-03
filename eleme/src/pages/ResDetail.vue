<!--//店家详情页-->

<template>
	<div class="page-after">
		<header>
			<span class="fa fa-arrow-left" @click="backClick"></span>
		</header>
		
		<ResDetailHeader :headerInfo="headerInfo"></ResDetailHeader>
		
		<Tab bar-active-color="#3190E8" v-model="pageIndex">
      <tab-item selected active-class="high-light">商品</tab-item>
      <tab-item active-class="high-light">评价</tab-item>
    </Tab>
    
    <MenuList v-show="pageIndex==0" :menuInfo="menuInfo"></MenuList>
    <Comment v-show="pageIndex==1"></Comment>
    
		
		<footer>
			
		</footer>
		
	</div>
</template>

<script>
	import bus from "../utils/bus.js";
	import ResDetailHeader from "../components/ResDetailHeader";
	import MenuList from "../components/MenuList";
	import Comment from "../components/Comment";
	import {Tab,TabItem} from 'vux';
	
	export default {
		components:{
			ResDetailHeader,
			Tab,
			TabItem,
			Comment,
			MenuList
		},
		data:function(){
			return {
				headerInfo:{},
				pageIndex:0,
				menuInfo:{},
			}
		},
		activated:function(){
			this.initData();
			this.requestHeaderInfo();
			this.requestMenuInfo();
		},
		methods:{
			backClick:function(){
				this.$router.back();
			},
			initData:function(){
				this.headerInfo = {};
				this.pageIndex = 0;
				this.menuInfo = {};
			},
			requestHeaderInfo:function(){
				this.$http.get("ele/shopping/restaurant/"+this.$route.params.resid,{
					params:{
						latitude:bus.latitude,
						longitude:bus.longitude,
						"extras":["activities","album","license","identification","statistics"]
					}
				})
				.then(function(res){
//					console.dir(res.data);
					this.headerInfo = res.data;
				}.bind(this));
			},
			requestMenuInfo:function(){
				this.$http.get("ele/shopping/v2/menu",{
					params:{
						restaurant_id:this.$route.params.resid
					}
				})
				.then(function(res){
					this.menuInfo = res.data;
				}.bind(this));
			}
		}
	}
</script>

<style scoped>
	header{
		height: 44px;
		background-color: #0096ff;
	}
	header span{
		font-size: 32px;
		line-height: 44px;
		margin-left: 8px;
		color: white;
	}
	.high-light{
		color: #3190E8 !important;
	}
	
	footer{
		height: 66px;
		background-color: #474747;
		position: absolute;
		left: 0;
		bottom: 0;
		width: 100%;
		display: flex;
		justify-content: space-between;
	}
</style>