<template>
	<div class='cart'>
		<div class='section_left' @click='showInfo'>
			<div class='logo_wrapper'>
				<div class='logo' :class='{ active: totalCount > 0 }'><i class='icon icon-shopping_cart'></i></div>
				<div class='count' v-show='totalCount > 0'>{{ totalCount }}</div>
			</div>
			<div class='desc_wrapper'>
				<div class='total_price' :class='{ active: totalPrice > 0 }'>{{ totalPrice | formatePrice }}</div>
				<div class='text'>另需配送费{{ seller.deliveryPrice | formatePrice }}元</div>
			</div>
		</div>
		<div class='section_right' :class='{ pay_active: payActive }'>{{ minDeliveryPrice }}</div>
		<div class='cart_info' v-show='infoIsShow' @click='hideInfo'>
			<div class='info_wrapper' @click.stop='testClick'>
				<div class='title'>
					<span class='text'>购物车</span>
					<span class='clear' @click='clearCart'>清空</span>
				</div>
				<div class='content' ref='content_scroll'>
					<ul class='select_food_list'>
						<li v-for='item in selectedFood' class='select_food_item'>
							<span class='name'>{{ item.name }}</span>
							<span class='price'>{{ item.price | formatePrice }}</span>
							<div class='add_wrapper'><add :food='item'></add></div>
						</li>
					</ul>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import add from '../add/add'
import BScroll from 'better-scroll'
export default {
	props: {
		seller: Object,
		selectedFood: Array
	},
	data(){
		return {
			infoIsShow: false
		}
	},
	methods: {
		showInfo(){
			this.infoIsShow = this.totalCount > 0;
		},
		clearCart(){
			this.selectedFood.forEach(item => {
				item.count = 0;
			})
		},
		hideInfo(){
			this.infoIsShow = !this.infoIsShow;
		},
		testClick(){
			return false;
		}
	},
	filters: {
		formatePrice(val){
			return `￥${val}`
		}
	},
	mounted(){
		this.$nextTick(() => {
			this.contentScroll = new BScroll(this.$refs.content_scroll, { click: true })
		})
	},
	computed: {
		totalPrice(){
			let total = 0;
			this.selectedFood.forEach(item => {
				total += item.price * item.count;
			})
			return total;
		},
		totalCount(){
			let totalCount = 0;
			this.selectedFood.forEach(item => {
				totalCount += item.count;
			})
			return totalCount;
		},
		minDeliveryPrice(){
			let diff = this.seller.minPrice - this.totalPrice;
			if(this.totalPrice === 0){
				return `￥${this.seller.minPrice}起送`
			}
			if(diff > 0){
				return `还差￥${diff}起送`
			}
			return `去结算`
		},
		payActive(){
			if(this.minDeliveryPrice === '去结算'){
				return true;
			}
			return false;
		}
	},
	components: {
		add
	}
}	
</script>

<style lang='sass' scoped>
.cart
	display: flex
	position: fixed
	z-index: 20
	bottom: 0
	left: 0
	width: 100%
	height: 48px	
	//background: rgba(7,17,27,0.95)
	.section_left
		flex: 1 1 auto
		display: flex
		background: rgba(7,17,27,0.95)
		.logo_wrapper
			background: rgba(7,17,27,0.95)
			padding: 6px
			margin: 0 12px
			position: relative
			top: -10px
			width: 44px
			height: 44px
			border-radius: 50%
			.logo
				display: flex
				justify-content: center
				align-items: center
				width: 44px
				height: 44px
				border-radius: 50%
				background: rgba(255,255,255,0.1)
				.icon
					font-size: 20px
					color: rgba(255,255,255,0.4)
				&.active
					background: #00a0dc
					.icon
						color: #fff	
			.count
				width: 24px
				height: 16px
				background: #f01414
				position: absolute
				top: 0
				right: 0
				color: #fff
				line-height: 16px
				text-align: center
				border-radius: 16px
				font-size: 9px
				font-weight: bold
				box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4)
		.desc_wrapper
			display: flex
			align-items: center
			color: rgba(255,255,255,0.4)
			//background: rgba(7,17,27,0.95)
			.total_price
				height: 24px
				font-size: 16px
				line-height: 24px
				font-weight: bold
				padding-right: 12px
				border-right: 1px solid rgba(255,255,255,0.1)
				&.active
					color: #fff
			.text
				padding-left: 12px
				font-size: 10px			
	.section_right
		flex: 0 0 auto	
		width: 105px
		line-height: 48px
		font-size: 12px
		color: rgba(255,255,255,0.4)
		text-align: center
		background: #2b333b
		font-weight: bold
		&.pay_active
			background: #00b43c
			color: #fff
	.cart_info
		position: fixed
		bottom: 0
		left: 0
		z-index: -1
		width: 100%
		height: 100%
		background: rgba(7,17,27,0.6)
		.info_wrapper
			position: absolute
			bottom: 0
			left: 0
			width: 100%
			height: auto
			padding-bottom: 70px
			background: #fff
			.title
				display: flex
				justify-content: space-between
				align-items: center
				padding: 0 18px
				width: 100%
				height: 39px
				box-sizing: border-box
				background: #f3f5f7
				border-bottom: 1px solid #dbdee1
				.text
					font-size: 14px
					color: #07111b
				.clear	
					font-size: 12px
					font-weight: 400
					color: #00a0dc
			.content	
				background: #fff
				max-height: 258px
				overflow: hidden
				.select_food_list
					padding: 0 18px
					.select_food_item
						display: flex
						align-items: center
						height: 48px
						border-bottom: 1px solid #e6e7e8
						.name
							flex: 1 1 auto
							font-size: 14px
							color: #07111b
							font-weight: 400
						.price
							flex: 0 0 auto
							font-size: 14px 
							color: #f01414
							font-weight: bold
							margin-right: 15px
						.add_wrapper
							flex: 0 0 auto
</style>