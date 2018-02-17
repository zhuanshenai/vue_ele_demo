<template>
	<div class='rating_select'>	
		<div class='btns_wraper'>
			<div class='positive btn' :class='{ selected: defaultSelect === 2 }' @click='changeSelected(2)'>{{ btnDescription.all }}<span class='count'>{{ ratings.length }}</span></div>
			<div class='positive btn' :class='{ selected: defaultSelect === 0 }' @click='changeSelected(0)'>{{ btnDescription.positive }}<span class='count'>{{ positiveRatings.length }}</span></div>
			<div class='negative btn' :class='{ selected: defaultSelect === 1 }' @click='changeSelected(1)'>{{ btnDescription.negative }}<span class='count'>{{ negativeRatings.length }}</span></div>
		</div>
		<div class='has_content_btn' :class='{on: isOnlyContent}' @click='toggleContent'>
			<i class='icon icon-check_circle'></i>
			<span class='text'>只看有内容的评价</span>
		</div>
	</div>
</template>

<script>
const ALL = 2;
const POSITIVE = 0;
const NEGATIVE = 1;
export default {
	props: {
		ratings: Array,  // 通过父组件传递过来的评价：产品详情的评价与商家评价
		btnDescription: { // 通过父组件传递过来按钮的名称：(全部/满意/不满意)、(全部/推荐/吐槽)
			type: Object,
			default(){
				return {
					all: '全部',
					positive: '满意',
					negative: '不满意'
				}
			}
		},
		defaultSelect: {
			type: Number,
			default: ALL
		},
		isOnlyContent: {
			type: Boolean,
			default: false
		} 
	},
	methods: {
		changeSelected(type){
			this.$emit('changeselected', type)
		},
		toggleContent(){
			this.$emit('togglecontent')
		}
	},
	created(){
		//console.log(this.defaultSelect)
	},
	computed: {
		positiveRatings(){
			return this.ratings.filter(item => {
				return item.rateType === POSITIVE;
			})
		},
		negativeRatings(){
			return this.ratings.filter(item => {
				return item.rateType === NEGATIVE;
			})
		}
	}
}	
</script>

<style scoped lang='sass'>
.rating_select
	.btns_wraper
		margin: 0 18px
		padding: 18px 0
		border-bottom: 1px solid #e6e7e8
		display: flex
		.btn
			padding: 10px 12px
			border-raidus: 1px 
			text-align: center
			font-size: 12px
			color: #4d555d
			margin-right: 8px
			&:last-child
				margin: 0
			.count
				font-size: 8px
				margin-left: 3px	
		.positive
			background: rgba(0,160,220,0.2)
			&.selected
				background: #00a0dc
				color: #fff		
		.negative	
			background: rgba(147,153,159,0.2)	
			&.selected
				background: #93999f
				color: #fff		
	.has_content_btn
		padding: 14px 20px		
		border-bottom: 1px solid rgba(7,17,27,0.1)
		font-size: 0
		&.on
			.icon
				color: #00c850
		.icon
			font-size: 20px
			color: #b7bbbf
			display: inline-block
			vertical-align: middle
		.text
			font-size: 12px
			color: #93999f
			display: inline-block
			vertical-align: middle
			margin-left: 6px
			font-weight: 400
</style>