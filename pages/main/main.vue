<template>
	<view>
		<u-divider  half-width="200">近期活动</u-divider>
		<u-swiper :list="list" interval="8000" title="true"></u-swiper>
		<u-divider  half-width="200">功能选择</u-divider>
		<u-grid :col="3">
			<u-grid-item>
				<navigator url="../NPcalc/NPcalc">
					<u-image width="127rpx" height="127rpx" :src="icon1"></u-image>
				</navigator>
				<view class="grid-text">宝具伤害</view>
			</u-grid-item>
			<u-grid-item>
				<navigator url="../CARDcalc/CARDcalc">
					<u-image width="127rpx" height="127rpx" :src="icon2"></u-image>
				</navigator>
				<view class="grid-text">色卡伤害</view>
			</u-grid-item>
		</u-grid>	
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
						image: 'https://fgo.wiki/images/c/c4/Apocrypha_Inheritance_of_Glory%E5%A4%8D%E5%88%BB.png',
						title: 'Apocrypha Inheritance of Glory复刻'
					},
					{
						image: 'https://fgo.wiki/images/1/15/%E7%A7%8B%E5%8F%B6%E5%8E%9F%E7%88%86%E7%A0%B4%EF%BC%81_jp.png',
						title: '秋叶原爆破！ ～心愿之街与铭刻着爱的雕像群～'
					},
				],
				icon1: 'https://fgo.wiki/images/4/4f/%E8%8A%A5%E9%9B%8F%E5%AD%90-%E4%BB%A4%E5%92%92.png',
				icon2: 'https://fgo.wiki/images/9/97/%E5%8D%A1%E5%A4%9A%E5%85%8B-%E4%BB%A4%E5%92%92.png'
			}
		},
		methods: {

		}
	}
</script>

<style lang="scss">
	.wrap {
		padding: 40rpx;
	}
</style>
