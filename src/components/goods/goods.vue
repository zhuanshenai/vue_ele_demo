<template>
	<div class='goods'>
		<div class='menu_wrapper' ref='menu_scroll'>
			<ul class='menu_list'>
				<li class='menu_item' v-for='(item,index) in goods' :class='{ active: (index === currentIndex) }' @click='contentIntoView(index)'>
					<p class='text'><span class='img' v-if='item.type > 0' :class='typeLists[item.type]'></span>{{ item.name }}</p>
				</li>
			</ul>
		</div>
		<div class='content_wrapper' ref='content_wrapper'>
			<div class='content'>
				<div class='category_list' v-for='good in goods' ref='category_list'>
					<h2 class='title'>{{ good.name }}</h2>
					<ul class='food_wrapper'>
						<li class='food_list' v-for='food in good.foods'>
							<img :src='food.icon' class='img'>
							<div class='desc_wrapper'>
								<h3 class='name'>{{ food.name }}</h3>
								<h4 class='category'>{{ food.description }}</h4>
								<p class='sell'>月售{{ food.sellCount }}份<span class='rating'>好评率{{ food.rating }}%</span></p>
								<p class='price'>{{ food.price | formatePrice }}<span class='old_price' v-if='food.oldPrice'>{{ food.oldPrice | formatePrice }}</span></p>
							</div>
							<div class='add_wrapper'>
								<add :food='food'></add>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</div>
		<cart :seller='seller' :selected-food='selectedFood'></cart>
	</div>
</template>

<script>
import BScroll from 'better-scroll'
import add from '../add/add'
import cart from '../cart/cart'
export default {
	props: {
		seller: Object,
		goods: Array
	},
	data(){
		return {
			typeLists: [ 'decrease', 'discount', 'special' ],
			scrollY: 0,
			listHeight: []
		}
	},
	computed: {
		selectedFood(){
			let results = [];
			this.goods.forEach(item => {
				item.foods.forEach(food => {
					if(food.count){
						results.push(food)
					}
				})
			})
			return results;
		},
		currentIndex(){
			for(let i = 0; i < this.listHeight.length; i++){
				let height1 = this.listHeight[i];
				let height2 = this.listHeight[i + 1];
				if((this.scrollY >= height1 && this.scrollY < height2) || !height2){
					return i;
				}
			}
			return 0;
		}
	},
	watch: {
		goods(){
			this.$nextTick(() => {
				let height = 0;
				this.listHeight.push(height)
				this.$refs.category_list.forEach(item => {
					height += item.clientHeight;
					this.listHeight.push(height)
				})
			})
		}
	},
	methods: {
		contentIntoView(index){
			console.log(index)
			this.contentScroll.scrollToElement(this.$refs.category_list[index], 400)
		}
	},
	mounted(){
		this.menuScroll = new BScroll(this.$refs.menu_scroll, { click: true });
		this.contentScroll = new BScroll(this.$refs.content_wrapper, { click: true, probeType: 3 });
		this.contentScroll.on('scroll', (pos) => {
			this.scrollY = Math.abs(Math.floor(pos.y));
		})
	},
	components: {
		add,
		cart
	},
	filters: {
		formatePrice(val){
			return `￥${val}`;
		}
	}
}	
</script>

<style scoped lang='sass'>
@import '../../assets/bg.sass'
.goods	
	display: flex
	height: 445px
	.menu_wrapper
		flex: 0 0 auto
		width: 80px
		height: 100%
		overflow: hidden
		background: #f3f5f7
		.menu_list
			.menu_item 
				display: table
				width: 100%
				height: 54px
				padding: 0 12px
				box-sizing: border-box
				&.active 
					background: #fff
					position: relative
					top: -1px
					.text
						font-weight: 400
						border: none
				.text
					display: table-cell
					vertical-align: middle
					border-bottom: 1px solid #dbdee1
					font-size: 12px
					line-height: 14px
					color: #07111b
					.decrease
						@include bg('decrease_3')
					.discount
						@include bg('discount_3')
					.special
						@include bg('special_3')
					.img
						display: inline-block
						vertical-align: top
						width: 12px
						height: 12px
						margin-right: 2px
						background-size: 12px 12px
	.content_wrapper
		flex: 1 1 auto
		height: 100%
		overflow: hidden
		background: #fff
		.content
			.category_list
				.title
					width: 100%
					height: 26px
					background: #f3f5f7
					color: #93999f
					font-size: 12px
					line-height: 26px
					padding-left: 12px
					border-left: 2px solid #d9dde1
					box-sizing: border-box
				.food_wrapper	
					padding: 0 18px
					.food_list
						padding: 18px 0
						display: flex
						border-bottom: 1px solid #d9dde1
						position: relative
						&:last-child
							border: none
						.img
							width: 57px
							height: 57px
							border-radius: 2px
						.desc_wrapper	
							flex: 1 1 auto
							margin-left: 9px
							.name
								font-size: 14px
								color: #07111b
								font-weight: 400
								margin: 2px 0 6px
							.category
								font-size: 10px
								color: #93999f
								font-weight: 400
								line-height: 14px
							.sell
								font-size: 10px
								color: #93999f
								font-weight: 400
								line-height: 14px
								margin: 4px 0
								.rating
									margin-left: 12px
							.price
								font-size: 14px
								font-weight: bold
								color: #f01414
								.old_price
									font-size: 10px
									line-height: 14px
									color: #93999f
									font-weight: bold
									text-decoration: line-through	
									margin-left: 8px		
						.add_wrapper
							position: absolute
							right: 2px
							bottom: 14px			
</style>