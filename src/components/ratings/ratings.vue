<template>
	<div class='ratings' ref='ratings_scroll'>
		<div class='scroll_wraper_hook'>
			<div class='section_top'>
				<div class='side'>
					<h1 class='score'>{{ seller.score }}</h1>
					<h2 class='text'>综合评分</h2>
					<p class='rank'>高于周边商家{{ seller.rankRate }}%</p>
				</div>
				<div class='star_wraper'>
					<div class='service'>
						<p class='text'>服务态度</p>
						<star :score='seller.serviceScore' :size='36'></star>
					</div>
					<div class='food_score'>
						<p class='text'>商品评分</p>
						<star :score='seller.foodScore' :size='36'></star>
					</div>
					<div class='delivery_time'>
						<p class='text'>送达时间</p>
						<span class='timing'>{{ seller.deliveryTime }}分钟</span>
					</div>
				</div>
			</div>
			<gutter></gutter>
			<rating-select :ratings='ratings' :btn-description='btnDescription' :default-select='defaultSelect' :is-only-content='isOnlyContent' @changeselected='change' @togglecontent='toggle'></rating-select>
			<div class='commet_wraper' v-if='ratings.length > 0'>
				<ul>
					<li class='commet_content' v-for='item in ratings' v-show='selectContent(item.rateType, item.text)'>
						<div class='avatar'>
							<img class='img' :src='item.avatar' width='28' height='28'>
						</div>
						<div class='content'>
							<h2 class='name'>{{ item.username }}</h2>
							<div class='star_wraper'>
								<star :size='24' :score='item.score'></star>
								<p class='delivery_time' v-if='item.deliveryTime'>{{ item.deliveryTime }}分钟送达</p>
							</div>
							<h3 class='text'>{{ item.text }}</h3>
							<div class='recommend' v-if='item.recommend.length > 0'>
								<i class='icon' :class='{"icon-thumb_up": item.rateType === 0, "icon-thumb_down": item.rateType === 1}'></i>
								<span v-for='recom in item.recommend' class='recommend_item'>{{ recom }}</span>
							</div>
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
const ALL = 2;
const POSITIVE = 0;
const NEGATIVE = 1
import star from '../star/star'
import gutter from '../gutter/gutter'
import ratingSelect from '../ratingSelect/ratingSelect'
import BScroll from 'better-scroll'
export default {
	data(){
		return {
			btnDescription: {
				all: '全部',
				positive: '满意',
				negative: '不满意'
			},
			defaultSelect: ALL,
			isOnlyContent: false
		}
	},
	props: {
		seller: Object,
		ratings: Array
	},
	methods: {
		selectContent(type, text){
			if(this.isOnlyContent && !text){
				return false;
			}
			if(this.defaultSelect === ALL){
				return true;
			}else{
				return type === this.defaultSelect
			}
		},
		toggle(){
			this.isOnlyContent = !this.isOnlyContent;
		},
		change(type){
			this.defaultSelect = type;
		}
	},
	watch: {
		ratings(){
			this.ratingsCopy = this.ratings;
		}
	},
	created(){
		this.$nextTick(() => {
			this.ratingsScroll = new BScroll(this.$refs.ratings_scroll, { click: true })
		})
	},
	components: {
		star,
		gutter,
		ratingSelect
	}
}	
</script>

<style lang='sass' scoped>
.ratings
	height: 493px
	overflow: hidden
	.scroll_wraper_hook
		.section_top
			display: flex
			padding: 18px
			.side
				flex: 0 0 120px
				display: flex
				flex-direction: column
				justify-content: space-around
				width: 120px
				height: 70px
				text-align: center
				border-right: 1px solid #e6e7e8
				box-sizing: border-box
				font-weight: 400
				.score
					font-size: 24px
					line-height: 14px
					color: #ff9900
				.text
					font-size: 12px
					line-height: 14px
					color: #07111b
				.rank
					font-size: 10px
					line-height: 14px
					color: #93999f	
			.star_wraper
				flex: 1
				height: 70px
				display: flex
				flex-direction: column
				justify-content: space-around
				margin-left: 24px
				.service, .food_score, .delivery_time
					font-size: 12px
					line-height: 14px
					color: #07111b
					display: flex
					align-items: center
					.text
						margin-right: 12px	
					.timing
						color: #93999f		
		.commet_wraper
			padding: 0 18px
			.commet_content	
				padding: 18px 0
				border-bottom: 1px solid #e6e7e8	
				position: relative		
				display: flex
				.avatar
					flex: 0
					margin-right: 12px
					.img
						border-radius: 50%
				.content
					.name
						font-size: 10px
						line-height: 14px
						color: #07111b
						font-weight: 400
					.star_wraper
						display: flex	
						.delivery_time
							font-size: 10px
							line-height: 14px
							color: #93999f
							margin-left: 8px
					.text
						font-size: 12px
						line-height: 18px
						color: #07111b
						font-weight: 400	
					.recommend
						display: flex
						align-items: center
						flex-wrap: wrap
						.icon
							margin-right: 8px
							color: #b7bbbf
						.icon-thumb_up
							color: #00a0dc
						.recommend_item
							padding: 3px 6px
							font-size: 9px
							line-height: 18px
							color: #93999f
							border: 1px solid #e6e7e8
							margin-right: 8px
							display: inline-block
							max-width: 62px
							box-sizing: border-box
							overflow: hidden
							text-overflow: ellipsis
							white-space: nowrap
							margin-bottom: 3px
							&:last-child
								margin: 0		
</style>