<template>
	<div class='supports'>
		<ul :class='supportType'>
			<li class='icon_info' v-for='support in supports'>
				<span class='icon' :class='typeMaps[support.type]'></span>
				<span class='text'>{{ support.description }}</span>
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	props: {
		supports: Array,
		supportType: String // 组件是在header中还是seller中
	},
	data(){
		return {
			typeMaps: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
		}
	}
}	
</script>

<style lang='sass' scoped>
@import '../../assets/bg.sass'
.supports	
	.seller
		.decrease
			@include bg('decrease_4')
		.discount
			@include bg('discount_4')	
		.special
			@include bg('special_4')		
		.invoice
			@include bg('invoice_4')
		.guarantee
			@include bg('guarantee_4')
	.header
		.decrease
			@include bg('decrease_2')
		.discount
			@include bg('discount_2')	
		.special
			@include bg('special_2')		
		.invoice
			@include bg('invoice_2')
		.guarantee
			@include bg('guarantee_2')
	.seller, .header
		.icon_info
			width: 100%
			height: 16px
			display: flex
			align-items: center	
			.icon
				display: inline-block
				width: 16px
				height: 16px
				background-size: 16px 16px			
</style>