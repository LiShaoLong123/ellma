<!--//菜单页面组件-->

<template>
	<div class="root">
		
		<div class="left">
			<div v-for="(c,i) in menuInfo" :class="{'select-option':i==selectedIndex}" class="category-item" @click="cateItemClick(i+1)">
				<img v-if="c.icon_url" :src="c.icon_url|isf"/>{{c.name}}
			</div>
		</div>
		
		<div class="right" ref="rightbox" @scroll="rightScroll">
			<div class="menu-group" v-for="(c,i) in menuInfo" :ref="i+1">
				<div class="menu-title hide-text">
					<span class="title-name">{{c.name}}</span>
					<span class="title-des">{{c.description}}</span>
				</div>
				<div class="group-item" v-for="f in c.foods">
					<img v-if="f.image_path" class="food-img" :src="f.image_path|isf"/>
					<div class="food-info">
						<h3 class="f-name">{{f.name}}</h3>
						<div v-if="f.description" class="f-des">{{f.description}}</div>
						<div v-if="f.month_sales||f.satisfy_rate" class="f-sale-rat">
							<span v-if="f.month_sales">月售{{f.month_sales}}份</span>
							<span v-if="f.satisfy_rate">好评率{{f.satisfy_rate}}%</span>
						</div>
						<div
							v-if="f.activity" 
							class="f-activity" 
							:style="{color:'#'+f.activity.icon_color,border:'solid 1px '+'#'+f.activity.icon_color}">
							{{f.activity.image_text}}
						</div>
						<div class="f-price">
							<span>
								<span 
									class="price-label current-price"
									:class="{multiple:(f.specfoods.length>1)}"
								>{{f.specfoods[0].price}}</span>
								<template v-if="f.specfoods[0].original_price">
									<del class="price-label original-price">{{f.specfoods[0].original_price}}</del>
									<span class="last-count">仅剩{{f.specfoods[0].stock}}份</span>
								</template>
							</span>
							
							<span class="add-order" v-if="f.specfoods[0].stock>0">
								<span v-if="f.specifications.length">选规格</span>
								<span v-else>＋</span>
							</span>
							<span class="sold-out" v-if="f.specfoods[0].stock<=0">已售完</span>
							
							
						</div>
					</div>
				</div>
			</div>
		</div>
		
	</div>
</template>

<script>
	import scrollAnimate from "../utils/scrollAnimate.js";
	export default {
		props:["menuInfo"],
		data:function(){
			return{
				selectedIndex:0,
				offsetTable:[]
			}
		},
		methods:{
			cateItemClick:function(index){
				var cg = this.$refs[index][0];
				var offset = cg.offsetTop - cg.parentNode.offsetTop;
				var rightbox = this.$refs.rightbox;
				scrollAnimate(rightbox,offset,200);
				this.selectedIndex = index-1;
			},
			rightScroll:function(e){
//				console.log(e.target.scrollTop);
				for (var i = this.offsetTable.length-1;i>=0;i--){
					if(e.target.scrollTop>=this.offsetTable[i]){
						this.selectedIndex = i;
						break;
					}
				}
			}
		},
		updated:function(){
			this.offsetTable = [];
//			console.log(this.$refs);
			for (var i = 1;i<=this.menuInfo.length;i++) {
				var groupItem = this.$refs[i][0];
//				console.log(groupItem);
				this.offsetTable.push(groupItem.offsetTop - groupItem.parentNode.offsetTop);
			}
			
//			console.log(this.offsetTable);
		},
	}
</script>

<style scoped>
	.root{
		width: 100%;
		height: calc(100vh - 274px);
		display: flex;
	}
	.left{
		width: 22%;
		overflow-y: scroll;
	}
	.right{
		width: 78%;
		overflow-y: scroll;
	}
	
	.category-item{
		border-bottom: solid 1px lightgrey;
		text-align: center;
		line-height: 70px;
		font-size: 1rem;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		background-color: #F8F8F8;
		color: #666666;
	}
	.category-item img{
		width: 1.2rem;
	}
	
	.menu-group{
		background-color: #f0f0f0;
	}
	
	.menu-title{
		height: 40px;
		line-height: 40px;
		background-color: #F1F1F1;
		padding: 0 8px;
	}
	
	.title-name{
		font-size: 1.2rem;
		font-weight: bold;
		color: #666666;
	}
	
	.title-des{
		color: #999999;
		font-size: 0.8rem;
	}
	
	.group-item{
		/*height: 180px;*/
		border-bottom: solid 1px #F1F1F1;
		background-color: white;
		display: flex;
		padding: 20px 10px;
	}
	
	.food-img{
		width: 5rem;
		height: 5rem;
		border-radius: 4px;
	}
	
	.food-info{
		flex-grow: 1;
		margin-left: 8px;
	}
	
	.f-name{
		margin: 0;
	}
	
	.f-des{
		margin: 4px 0;
		font-size: 0.8rem;
		color: #999999;
	}
	.f-sale-rat{
		margin: 4px 0;
		font-size: 0.9rem;
		color: #666666;
	}
	
	.f-activity{
		margin: 4px 0;
		display: inline-block;
		padding: 2px;
		font-size: 0.8rem;
		border-radius: 2px;
	}
	
	.current-price{
		color: #FF6600;
		font-size: 1.4rem;
		font-weight: bold;
	}
	
	.price-label:before{
		content: "￥";
	}
	
	.original-price{
		color: #999999;
	}
	
	.last-count{
		font-size: 0.8rem;
		color: white;
		font-weight: bold;
		background-color: red;
		border-radius: 2px;
		padding: 0 2px;
	}
	.multiple:after{
		content: "起";
		font-size: 1rem;
		color: #666666;
		font-weight: 100;
	}
	
	.add-order{
		float: right;
		background-color: #3199E8;
		color: white;
		border-radius: 1rem;
		font-size: 1rem;
		padding: 4px;
	}
	
	.sold-out{
		float: right;
		color: #666666;
	}
	
	.select-option{
		border-left: solid 4px #0096FF;
		background-color: white;
		
	}
</style>