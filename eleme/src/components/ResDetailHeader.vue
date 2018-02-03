<!--//店家详情页面头部组件-->

<template>
	<div style="height: 120px;">
		<Blur :blur-amount="40" :url="headerInfo.image_path|isf" :height="120">
      <div class="blur-box">
      	
      	<div class="top">
      		<img :src="headerInfo.image_path|isf"/>
      		<div class="top-right">
      			<div class="trt">{{headerInfo.name}}</div>
      			<div class="trm">
      				<span v-if="headerInfo.delivery_mode" class="pre-g">蜂鸟转送</span>
      				<span v-else>商家配送</span>
      				<span class="pre-g">{{headerInfo.order_lead_time}}分钟送达</span>
      				<span class="pre-g">满70免配送费</span>
      				<span style="float: right;">></span>
      			</div>
      			<div class="trb">
      				{{headerInfo.promotion_info||'公告：欢迎光临，用餐高峰期请提前下单，谢谢。'}}
      			</div>
      		</div>
      	</div>
      	
      	<div class="bottom" v-if="headerInfo.activities">
      		<div class="bl">
      			<span :style="{backgroundColor:'#'+headerInfo.activities[0].icon_color}">{{headerInfo.activities[0].icon_name}}</span>
      			<span class="huodong">{{headerInfo.activities[0].description}}</span>
      		</div>
      		
      		<span class="br">3个活动</span>
      	</div>
      	
      </div>
    </Blur>
	</div>
</template>

<script>
	import {Blur} from 'vux';
	
	export default {
		components:{
			Blur
		},
		props:["headerInfo"],
		
	}
</script>

<style scoped>
	.blur-box{
		padding: 8px;
	}
	
	.top{
		display: flex;
	}
	
	.top img{
		flex-basis: 80px;
	}
	
	.top-right{
		
		width: calc(100% - 80px);
		margin-left: 8px;
		display: flex;
		flex-direction: column;
	}
	
	.top-right>div{
		
	}
	
	.trt{
		flex-basis: 40%;
		font-size: 20px;
		font-weight: bold;
		color: white;
	}
	
	.trm{
		flex-basis: 30%;
		margin-top: 8px;
		color: white;
		font-size: 14px;
	}
	
	.pre-g:before{
		content:"/";
	}
	
	.trb{
		width: 100%;
		flex-basis: 30%;
		margin-top: 8px;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		color: white;
		font-size: 14px;
	}
	
	.bottom{
		margin-top: 8px;
		color: white;
		display: flex;
		font-size: 14px;
	}
	
	.bl{
		width: calc(100% - 60px);
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
	}
	
	.br{
		width: 60px;
	}
	
</style>