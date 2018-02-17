<template>
	<div class='header'>
		<div class='section_top'>
			<div class='avatar'>
				<img :src='seller.avatar' width='64' height='64'>
			</div>
			<div class='content'>
				<div class='brand'>
					<span class='img'></span>
					<h1 class='name'>{{ seller.name }}</h1>
				</div>
				<div class='delivery'>
					<p class='text'>{{ seller.description }}/{{ seller.deliveryTime }}分钟送达</p>
				</div>
				<div class='support' v-if='supports.length > 0'>
					<span class='img' :class='supportsMap[supports[0].type]'></span>
					<p class='text'>{{ supports[0].description }}</p>
				</div>
				<div class='count' v-if='supports.length > 0' @click='showBulletin'>
					{{ supports.length }}个<i class='icon icon-keyboard_arrow_right'></i>
				</div>
			</div>
		</div>
		<div class='bulletin' v-if='seller.bulletin' @click='showBulletin'>
			<span class='img'></span>
			<p class='text'>{{ seller.bulletin }}</p>
			<i class='icon icon-keyboard_arrow_right'></i>
		</div>
		<div class='popup' v-show='showPopup'>
			<div class='name'>{{ seller.name }}</div>
			<div class='star_wraper'>
				<star :size='48' :score='seller.score'></star>
			</div>
			<div class='discount_info'>
				<div class='content_wraper'>
					<span class='line'></span>
					<h2 class='text'>优惠信息</h2>
					<span class='line'></span>
				</div>
				<supports :supports='supports' :supportType='supportType'></supports>
			</div>
			<div class='announce'>
				<div class='content_wraper'>
					<span class='line'></span>
					<h2 class='text'>商家公告</h2>
					<span class='line'></span>
				</div>	
				<p class='info'>{{ seller.bulletin }}</p>
			</div>
			<div class='close' @click='showBulletin'>
				<i class='icon icon-close'></i>
			</div>
		</div>
	</div>
</template>

<script>
import star from '../star/star'
import supports from '../supports/supports'
export default {
	props: {
		seller: Object
	},
	data(){
		return {
			supports: [],
			supportsMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
			supportType: 'header',
			showPopup: false
		}
	},
	watch: {
		seller(){
			this.supports = this.seller.supports
		}
	},
	components: {
		star,
		supports
	},
	methods: {
		showBulletin(){
			this.showPopup = !this.showPopup;
		}
	}
}	
</script>

<style lang='sass' scoped>
@import '../../assets/bg.sass'
.header
	width: 100%
	height: 134px
	background: #666	
	.section_top
		height: 106px
		padding: 24px 12px 18px 24px
		box-sizing: border-box
		display: flex
		position: relative
		.avatar
			border-radius: 2px
			overflow: hidden
			margin-right: 16px
		.content
			margin-top: 2px
			color: #fff
			.brand
				display: flex
				align-items: center
				.img
					@include bg('brand')
					display: inline-block
					width: 30px
					height: 18px
					background-size: 30px 18px
					margin-right: 6px
				.name
					font-size: 16px
					font-weight: bold	
			.delivery
				margin: 8px 0 10px
				.text
					font-size: 12px
			.support
				display: flex
				align-items: center
				.decrease 
					@include bg('decrease_1')
				.discount
					@include bg('discount_1')
				.special
					@include bg('special_1')
				.invoice
					@include bg('invoice_1')
				.guarantee
					@include bg('guarantee_1')
				.img
					display: inline-block
					width: 12px
					height: 12px
					background-size: 12px 12px
				.text
					font-size: 10px		
			.count
				position: absolute	
				right: 12px
				bottom: 15px
				background: #393839
				width: 43px
				height: 24px
				border-radius: 16px
				font-size: 10px
				display: flex
				align-items: center
				justify-content: flex-end
				.icon
					margin: 0 4px 0 1px
	.bulletin
		display: flex
		align-items: center
		color: #fff
		width: 100%
		padding: 0 12px
		box-sizing: border-box
		height: 28px
		background: rgba(7,17,27,0.2)//#07111b
		.img
			@include bg('bulletin')
			flex: 0 1 auto
			display: inline-block
			width: 22px
			height: 12px
			background-size: 22px 12px
			margin-right: 4px
		.text
			flex: 1
			overflow: hidden
			text-overflow: ellipsis
			white-space: nowrap
			font-size: 10px
		.icon		
			flex: 0 1 auto
			margin-left: 6px
	.popup
		position: absolute
		top: 0
		left: 0
		width: 100%
		height: 100%
		background: rgba(7,17,27,0.8)
		color: #fff
		z-index: 20		
		padding: 64px 36px 0 36px
		box-sizing: border-box
		.name
			font-size: 16px
			font-weight: bold
			text-align: center
			margin-bottom: 20px
		.discount_info
			margin: 28px 0
		.discount_info, .announce	
			.content_wraper
				display: flex
				align-items: center
				margin-bottom: 24px
				.line
					flex: 1 1 auto
					display: inline-block
					height: 1px
					background: rgba(255,255,255,0.2)
				.text
					font-size: 14px
					font-weight: bold
					margin: 0 12px
		.announce
			.info
				font-size: 12px
				line-height: 24px
				padding: 0 12px			
		.close
			text-align: center
			margin-top: 110px
			.icon
				font-size: 20px
				color: rgba(255,255,255,0.5)		
</style>

<style lang='sass'>
// supports 组件在header父组件中的样式
.header
	height: 128px
	display: flex
	flex-direction: column
	justify-content: space-between
	.icon_info
		margin-left: 12px
		.icon
			margin-right: 6px
		.text
			font-size: 12px
</style>