<template>
	<div class='add'>
		<div class='decrease' v-show='food.count > 0'><i class='icon icon-remove_circle_outline' @click='decreaseCount'></i></div>
		<div class='num' v-show='food.count > 0'>{{ food.count }}</div>
		<div class='increase' @click='increaseCount'><i class='icon icon-add_circle'></i></div>
	</div>
</template>

<script>
import Vue from 'vue'
export default {
	props: {
		food: Object
	},
	created(){
		//console.log(this.food.count)
	},
	methods: {
		increaseCount(){
			if(!this.food.count){
				Vue.set(this.food, 'count', 1)
			}else{
				this.food.count++;
			}
			this.$emit('increase');
			
		},
		decreaseCount(){
			this.food.count === 0 ? 0 : this.food.count--
		}
	}
}	
</script>

<style lang='sass' scoped>
.add
	display: flex
	.num
		width: 28px
		height: 20px
		line-height: 20px
		text-align: center
		font-size: 10px
		color: #93999f
	.decrease, .increase	
		.icon
			font-size: 20px
			color: #00a0dc
</style>