<template>
	<div class='cart'>
		<div class='section_left'>
			<div class='cart_icon_wraper'>
				<div class='cart_icon' :class='{hightlight: totalCount > 0}'>
					<i class='icon icon-shopping_cart'></i>
				</div>
				<div class='num' v-show='totalCount > 0'>{{ totalCount }}</div>
			</div>
			<div class='total_price' :class='{hightlight: totalPrice > 0}'>{{ totalPrice | formatPrice }}</div>
			<div class='description'>另需配送费{{ seller.deliveryPrice | formatPrice }}元</div>
		</div>
		<div class='section_right' :class='{active: totalPrice >= seller.minPrice}'>
			{{ delivery }}
		</div>
	</div>
</template>

<script>
export default {
	props: {
		seller: Object,
		selectFoods: {
			type: Array,
			default(){
				return []
			}
		}
	},
	computed: {
		totalPrice(){
			let totalPrice = 0;
			this.selectFoods.forEach(item => {
				totalPrice += item.price*item.count
			})
			return totalPrice
		},
		totalCount(){
			let count = 0;
			this.selectFoods.forEach(item => {
				count += item.count;
			})
			return count;
		},
		delivery(){
			let minPrice = this.seller.minPrice;
			if(this.totalPrice === 0){
				return '￥20起送';
			}
			if(this.totalPrice < minPrice){
				let diff = minPrice - this.totalPrice;
				return `还差￥${diff}起送`
			}else{
				return '去结算'
			}
		}
	},
	filters: {
		formatPrice(val){
			return `￥${val}`
		}
	}
}	
</script>

<style scoped lang='sass'>
.cart
	display: flex
	width: 100%
	height: 48px
	background: rgba(7,17,27,0.95)
	position: fixed
	bottom: 0
	left: 0	
	.section_left
		flex: 1 1 auto
		display: flex
		position: relative
		.cart_icon_wraper
			width: 56px
			height: 56px
			border-radius: 50%
			background: rgba(7,17,27,0.95)
			margin: 0 12px
			padding: 12px
			box-sizing: border-box
			position: relative
			top: -10px
			.cart_icon
				display: flex
				align-items: center
				justify-content: center
				width: 100%
				height: 100%
				border-radius: 50%
				background: rgba(255,255,255,0.1)
				&.hightlight
					background: #00a0dc
					.icon
						color: #fff
				.icon
					font-size: 20px
					color: rgba(255,255,255,0.4)
			.num
				position: absolute
				top: 0
				right: 0
				width: 24px
				height: 16px
				line-height: 16px
				color: #fff
				background: #f01414
				text-align: center
				box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4)	
				border-radius: 16px	
				font-size: 9px
				font-weight: bold 
		.total_price
			font-size: 16px
			line-height: 24px
			margin: 12px 0
			padding-right: 12px
			color: rgba(255,255,255,0.4)	
			border-right: 1px solid rgba(255,255,255,0.1)
			font-weight: bold
			&.hightlight
				color: #fff		
		.description
			height: 100%
			line-height: 48px
			font-size: 10px
			color: rgba(255,255,255,0.4)	
			margin-left: 12px
	.section_right	
		flex: 0 0 auto
		width: 105px
		background: #2b333b
		color: rgba(255,255,255,0.4)
		font-weight: bold
		font-size: 12px
		text-align: center
		line-height: 48px
		&.active
			background: #00b43c
			color: #fff
</style>