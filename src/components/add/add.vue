<template>
	<div class='add'>
		<div class='decrease' v-show='food.count > 0' @click.stop='decrease'><i class='icon icon-remove_circle_outline'></i></div>
		<div class='num' v-show='food.count > 0'>{{ food.count }}</div>
		<div class='increase' @click.stop='increase'><i class='icon icon-add_circle'></i></div>
	</div>
</template>

<script>
export default {
	props: {
		food: Object
	},
	methods: {
		increase(){
			if(!this.food.count){
				this.$set(this.food, 'count', 1)
			}else{
				this.food.count++
			}
		},
		decrease(){
			this.food.count > 0 ? this.food.count-- : 0
		}
	}
}	
</script>

<style lang='sass' scoped>
.add
	display: flex
	align-items: center
	.decrease, .increase
		font-size: 20px
		color: #00a0dc
		box-sizing: border-box
	.num
		font-size: 10px
		color: #93999f
		width: 28px		
		height: 20px
		line-height: 20px
		text-align: center
</style>