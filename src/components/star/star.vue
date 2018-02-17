<template>
	<div class='star'>
		<ul :class='starType'>
			<li v-for='star_class in starClasses' :class='star_class' class='star_item'></li>
		</ul>
	</div>
</template>

<script>
const LEN = 5;
const ON = 'on';
const HALF = 'half';
const OFF = 'off';
export default {
	props: {
		size: Number,
		score: Number
	},
	computed: {
		starType(){
			return `star_${this.size}`
		},
		starClasses(){
			let results = [];
			let score = Math.floor(this.score * 2) / 2;
			let integer = Math.floor(score);
			let hasDecimal = score % 1 !== 0;
			for(let i = 0; i < integer; i++){
				results.push(ON)
			}
			if(hasDecimal){
				results.push(HALF)
			}
			for(let i = results.length; i < LEN; i++){
				results.push(OFF)
			}
			return results;
		}
	}
}	
</script>

<style lang='sass' scoped>
@import '../../assets/bg.sass'
.star	
	display: flex
	justify-content: center
	.star_48
		width: 186px
		height: 24px
		.on
			@include bg('star48_on')
		.half
			@include bg('star48_half')
		.off
			@include bg('star48_off')
		.star_item			
			width: 20px
			height: 20px
			background-size: 20px 20px
	.star_36
		width: 100px
		height: 18px
		.on
			@include bg('star36_on')
		.half
			@include bg('star36_half')
		.off
			@include bg('star36_off')
		.star_item			
			width: 15px
			height: 15px
			background-size: 15px 15px
	.star_24
		width: 64px
		height: 12px
		.on
			@include bg('star24_on')
		.half
			@include bg('star24_half')
		.off
			@include bg('star24_off')
		.star_item			
			width: 10px
			height: 10px
			background-size: 10px 10px
	.star_48, .star_36, .star_24
		display: flex
		justify-content: space-between
		align-items: center
</style>