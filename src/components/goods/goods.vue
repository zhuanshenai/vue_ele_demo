<template>
	<div class='goods'>
		<div class='menu_wraper' ref='menu_scroll'>
			<ul class='menu_list'>
				<li class='menu_item' v-for='(item, index) in goods' :class='{ active: currentIndex === index}'>
					<p class='text'>	<span class='img' v-if='item.type > 0' :class='listMaps[item.type]'></span>
						{{ item.name }}</p>
				</li>
			</ul>
		</div>
		<div class='good_wraper' ref='good_scroll'>
			<ul class='good_list'>
				<li class='good_item' v-for='item in goods' ref='good_item'>
					<h2 class='name'>{{ item.name }}</h2>
					<ul class='food_wraper' v-for='food in item.foods'>
						<li class='food_list'>
							<img class='img' :src='food.icon' width='57' height='57'>
							<div class='food_info'>
								<h3 class='desc'>{{ food.name }}</h3>
								<h4 class='category' v-if='food.description'>{{ food.description }}</h4>
								<p class='info'>
									<span class='sale'>月售{{ food.sellCount }}份</span>好评率{{ food.rating }}%
								</p>
								<p class='price'>{{ food.price | formatePrice }}<span class='old_price' v-if='food.oldPrice'>{{ food.oldPrice | formatePrice }}</span></p>
							</div>
							<div class='add_wraper'>
								<add :food='food' @increase='increasement'></add>
							</div>
						</li>
					</ul>	
				</li>
			</ul>
		</div>
    <cart :seller='seller' :select-foods='selectFoods'></cart>
		<!-- <div class='mask'></div> -->
	</div>
</template>

<script>
import BScroll from 'better-scroll'
import cart from '../cart/cart'
import add from '../add/add'
export default {
	props: {
		goods: Array,
		seller: Object
	},
	data(){
		return {
			listMaps: [ 'decrease', 'discount', 'special' ],
			listHeight: [],
			scrollY: 0
		}
	},
	watch: {
		goods(){
			this.$nextTick(() => {
				this.scroll();
				this.calcHeight();
			})
		}
	},
	created(){
		this.$nextTick(() => {
			this.menuScroll = new BScroll(this.$refs.menu_scroll, { click: true });
			this.foodScroll = new BScroll(this.$refs.good_scroll, { click: true, probeType: 3 });
		})
	},
	methods: {
		scroll(){
			this.menuScroll = new BScroll(this.$refs.menu_scroll, {});
			this.foodScroll = new BScroll(this.$refs.good_scroll, { probeType: 3 });
			this.foodScroll.on('scroll', (pos) => {
				if(pos.y <= 0){
					this.scrollY = Math.abs(Math.round(pos.y));
				}
			})
		},
		calcHeight(){
			let goodItem = this.$refs.good_item;
			let height = 0;
			this.listHeight.push(height);
			for(let i = 0; i < goodItem.length; i++){
				let item = goodItem[i];
				height += item.clientHeight;
				this.listHeight.push(height)
			}
		},
		increasement(){
			//console.log(this.food.count)
			//this.selectFoods.push({ price: this.food.price, count: this.food.count})
		}
	},
	computed: {
		currentIndex(){
			for(let i = 0; i < this.listHeight.length; i++){
				let height1 = this.listHeight[i];
				let height2 = this.listHeight[i+1];
				if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
					return i;
				}
			}
			return 0;
		},
		selectFoods(){
			let foods = [];
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food);
          }
        });
      });
      return foods;
		}
	},
	filters: {
		formatePrice(val){
			return `￥${val}`
		}
	},
	components: {
		cart,
		add
	}
}	
</script>

<style lang='sass' scoped>
@import '../../assets/bg.sass'
.goods
	display: flex
	.menu_wraper
		flex: 0 0 auto
		height: 445px
		overflow: hidden
		.menu_list
			display: flex
			flex-direction: column
			width: 80px
			background: #f3f5f7
			.menu_item	
				width: 100%
				height: 54px
				padding: 0 12px
				box-sizing: border-box
				display: table
				.text
					display: table-cell
					vertical-align: middle
					width: 100%
					height: 100%
					border-bottom: 1px solid #dbdee1
					box-sizing: border-box
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
						background-size: 12px 12px		
						margin-right: 2px	
					//[ 'decrease', 'discount', 'special' ]
			.active
				background: #fff
				.text
					color: #07111b
					font-weight: 400		
	.good_wraper
		flex: 1 1 auto
		height: 445px	
		overflow: hidden	
		.good_list
			.good_item
				.name
					width: 100%
					height: 26px
					background: #f3f5f7
					border-left: 2px solid #d9dde1
					font-size: 12px
					line-height: 26px
					color: #93999f	
					padding-left: 12px
					box-sizing: border-box	
				.food_wraper
					padding: 0 18px
					&:last-child
						.food_list
							border: none
					.food_list
						display: flex	
						width: 100%
						padding: 18px 0
						border-bottom: 1px solid #d9dde1
						position: relative
						.img
							flex: 0 0 auto
							width: 57px
							height: 57px
							border-radius: 2px
							margin-right: 9px
						.food_info
							flex: 1 1 auto	
							.desc
								font-size: 14px
								color: #07111b
								font-weight: 400
								margin: 2px 0 8px
							.info	
								margin: 8px 0
								.sale
									margin-right: 12px
							.category, .info
								font-size: 10px
								color: #93999f
								line-height: 14px	
								font-weight: 400
							.price
								font-size: 14px
								color: #f01414	
								font-weight: bold
								.old_price
									font-size: 10px
									color: #93999f
									margin-left: 8px
									text-decoration: line-through											
						.add_wraper	
							position: absolute
							right: 0
							bottom: 19px			
	.mask
		width: 100%
		height: 100%
		position: fixed
		top: 0
		left: 0
		background: #999	
</style>