<template>
  <div id="app">
    <transition name="tabani">
	    <tabbar style="position: fixed;" v-show="isShowTabBar">
	    	<tabbar-item selected link="/main">
	    		<img slot="icon" src="../static/img/index1.png"/>
	    		<span slot="label">外卖</span>
	    	</tabbar-item>
	    	<tabbar-item link="/second">
	    		<img slot="icon" src="../static/img/index2.png"/>
	    		<span slot="label">发现</span>
	    	</tabbar-item>
	    	<tabbar-item>
	    		<img slot="icon" src="../static/img/index3.png"/>
	    		<span slot="label">订单</span>
	    	</tabbar-item>
	    	<tabbar-item>
	    		<img slot="icon" src="../static/img/index4.png"/>
	    		<span slot="label">我的</span>
	    	</tabbar-item>
	    </tabbar>
    </transition>
    
    
  	<transition :name="animationType">
  		<keep-alive>
    		<router-view></router-view>
    	</keep-alive>
    </transition>
    
    
    
  </div>
</template>

<script>
import {Tabbar,TabbarItem} from 'vux'
export default {
	components:{
		Tabbar,
		TabbarItem
	},
	data:function(){
		return {
			animationType:"",
			isShowTabBar:true
		}
	},
	mounted:function(){
//		console.log(this.$route);
		if(this.$route.path.split("/").length>2){
			this.isShowTabBar = false;
		}else{
			this.isShowTabBar = true;
		}
	},
	watch:{
		$route:function(to,from){
			var toNum = to.path.split("/").length;
			var fromNum = from.path.split("/").length;
			if(toNum==fromNum){
				//平级切换
				this.animationType = "qqq";
			}else if(toNum>fromNum){
				//进入
				this.animationType = "routein";
			}else{
				//退出
				this.animationType = "routeout";
			}
			
			if(toNum>2){
				this.isShowTabBar = false;
			}else{
				this.isShowTabBar = true;
			}
		}
	}
}

</script>

<style>
	
	
	
	/*进入动画*/
	.routein-enter-active{
		z-index: 10;
	}
	.routein-enter-active,.routein-leave-active{
		transition: all 0.3s;
		position: absolute;
	}
	.routein-enter{
		transform: translateX(100%);
	}
	.routein-leave-active{
		transform: translateX(-50%);
	}
	
	/*退出动画*/
	.routeout-enter-active,.routeout-leave-active{
		transition: all 0.3s;
		position: absolute;
	}
	.routeout-enter{
		transform: translateX(-50%);
	}
	.routeout-leave-active{
		transform: translateX(100%);
		z-index: 10;
	}
	
	/*tab动画*/
	.tabani-enter-active,.tabani-leave-active{
		transition: all 0.3s;
	}
	.tabani-enter,.tabani-leave-active{
		transform: translateY(100%);
	}
	
	
	.qqq-enter-active,.qqq-leave-active{
		position: absolute;
	}
	.qqq-enter{
		position: absolute;
	}
	
</style>


