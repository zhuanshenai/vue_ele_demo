<template>
	<div class='seller' ref='seller'>
		<div class='seller_wraper'>
			<div class='section_top'>
				<h1 class='name'>{{ seller.name }}</h1>
				<div class='star_wraper'>
					<star :score='seller.score' :size='36'></star>
					<span class='rating_count'>({{ seller.ratingCount }})</span>
					<span class='sell_count'>月售{{ seller.sellCount }}单</span>
				</div>
				<div class='info_wraper'>
					<div class='info'>
						<div class='info_item'>
							<h2 class='title'>起送价</h2>
							<p class='text'>{{ seller.minPrice }}<span>元</span></p>
						</div>
						<div class='info_item'>
							<h2 class='title'>商家配送</h2>
							<p class='text'>{{ seller.deliveryPrice }}<span>元</span></p>
						</div>
						<div class='info_item'>
							<h2 class='title'>平均配送时间</h2>
							<p class='text'>{{ seller.deliveryTime }}<span>分钟</span></p>
						</div>
					</div>
				</div>
				<div class='favorite' @click='favorite' :class='{active: isActive}'>
					<i class='icon icon-favorite'></i>
					<span class='text' ref='favorite_text'>收藏</span>
				</div>
			</div>
			<gutter></gutter>
			<div class='notice_wraper'>
				<h2 class='notice_title'>公告与活动</h2>
				<p class='notice_info'>{{ seller.bulletin }}</p>
				<supports :supports='seller.supports' :supportType='supportType'></supports>
			</div>
			<gutter></gutter>
			<div class='seller_pic'>
				<h2 class='pic_title'>商家实景</h2>
				<div class='pic_scroll' ref='pic_scroll'>
					<ul class='pic_wraper' ref='pic_wraper'>
						<li v-for='item in seller.pics' class='pic_items'>
							<img :src='item' width='120' height='90'>
						</li>
					</ul>
				</div>
			</div>
			<gutter></gutter>
			<div class='seller_info'>
				<h2 class='title'>商家信息</h2>
				<ul class='info_wraper'>
					<li class='info_item' v-for='item in seller.infos'>{{ item }}</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
import star from '../star/star'
import supports from '../supports/supports'
import gutter from '../gutter/gutter'
import BScroll from 'better-scroll'
export default {
	props: {
		seller: Object
	},
	components: {
		star,
		supports,
		gutter
	},
	data(){
		return {
			isActive: !!localStorage.getItem('isActive'), // 收藏按钮是否添加class=active
			supportType: 'seller'
		}
	},
	created(){
		this.$nextTick(() => {
			this._calc();
			this.sellerScroll = new BScroll(this.$refs.seller, { click: true });
			this.picScroll = new BScroll(this.$refs.pic_scroll, { click: true, scrollX: true, scrollY: false })
		})
	},
	watch: {
		seller(){
			this.$nextTick(() => {
				this._calc();
			})
		}
	},
	methods: {
		_calc(){
			let picWraper = this.$refs.pic_wraper;
			let totalWidth = 0;
			if(this.seller.pics){
				let pics = picWraper.getElementsByClassName('pic_items');
				[...pics].forEach(item => {
					let width = parseInt(window.getComputedStyle(item, null).width);
					let marginRight = parseInt(window.getComputedStyle(item, null).marginRight);
					totalWidth += width + marginRight;
				})
			}
			picWraper.style.width = totalWidth + 'px';
		},
		favorite(){
			//localStorage.setItem('isActive', !this.isActive)
			//console.log(localStorage.isActive, this.isActive)
			this.isActive = !this.isActive;
			localStorage.setItem('isActive', this.isActive);
			console.log(this.isActive)
			if(this.isActive){
				this.$refs.favorite_text.textContent = '已收藏'
			}else{
				this.$refs.favorite_text.textContent = '收藏'
			}
			//
		}
	}
}	
</script>

<style lang='sass' scoped>
.seller
	height: 493px
	overflow: hidden
	.seller_wraper
		.section_top
			padding: 18px
			position: relative
			.name
				font-size: 14px
				color: #07111b
				margin-bottom: 9px
				font-weight: 400
			.star_wraper
				display: flex
				align-items: center
				font-size: 10px
				font-weight: 400
				color: #4d555d
				padding-bottom: 18px
				border-bottom: 1px solid #e6e7e8
				.rating_count
					margin: 0 13px 0 9px
			.info_wraper
				padding-top: 18px
				.info
					height: 38px
					display: flex
					align-items: space-between
					.info_item
						flex: 1
						border-right: 1px solid #e6e7e8
						text-align: center
						&:last-child
							border: none
						.title
							font-size: 10px
							line-height: 14px
							color: #93999f
							font-weight: 400
						.text
							font-size: 24px
							color: #07111b
							span	
								font-size: 10px	
			.favorite
				width: 38px
				height: 40px
				position: absolute
				top: 18px
				right: 18px
				display: flex
				flex-direction: column
				justify-content: space-around
				align-items: center
				&.active
					.icon
						color: #f01414
					.text
						color: #4d555d	
				.icon
					font-size: 20px
					color: #d4d6d9
				.text					
					font-size: 10px
					line-height: 14px
					color: #93999f
					font-weight: 400
		.notice_wraper
			padding: 18px
				bottom: 0
			.notice_title
				font-size: 14px
				color: #07111b
				font-weight: 400
			.notice_info	
				font-size: 12px
				line-height: 24px
				color: #f01414
				padding: 9px 12px 15px
				border-bottom: 1px solid #e6e7e8					
		.seller_pic
			padding: 18px
			.pic_title
				font-size: 14px
				color: #07111b
				font-weight: 400
				margin-bottom: 12px
			.pic_scroll	
				overflow: hidden
				.pic_wraper
					display: flex
					.pic_items	
						width: 120px
						height: 90px
						margin-right: 6px	 
		.seller_info
			padding: 18px
			.title
				font-size: 14px
				color: #07111b
				font-weight: 400
				margin-bottom: 12px
			.info_wraper
				.info_item
					padding: 18px
					border-top: 1px solid #e6e7e8
					font-size: 12px
					line-height: 18px
					color: #07111b	
</style>

<style lang='sass'>
// supports 组件在seller父组件中的样式
.notice_wraper
	.icon_info
		padding: 16px 0 16px 12px	
		border-bottom: 1px solid #e6e7e8	
		&:last-child
			border-bottom: none
		.icon
			margin-right: 6px
		.text
			font-size: 12px
			color: #07111b
</style>