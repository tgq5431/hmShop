<template>
	<view >
		<view class="home">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
				<swiper-item v-for="item in swipers" :key=item.id>
					<image :src=item.img ></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="nav">
			<view class="nav_item">
				<view class="iconfont icon-ziyuan">
				</view>
				<text>黑马超市</text>
			</view>
			<view class="nav_item">
				<view class="iconfont icon-tupian">
					
				</view>
				<text>社区图片</text>
			</view>
			<view class="nav_item">
				<view class="iconfont icon-guanyuwomen">
					
				</view>
				<text>联系我们</text>
			</view>
			<view class="nav_item">
				<view class="iconfont icon-shipin">
					
				</view>
				<text>学习视频</text>
			</view>
		</view>
		
		<view class="shop">
			<view class="title">
				<text>推荐商品</text>
			</view>
			<view class="shop_list" >
				<view class="shop_item" v-for="item in shop" :key="item.id" >
					<image :src=item.img_url ></image>
					<text class="price">
						<span class="new_price">￥{{item.market_price}}</span>
						<span class="old_price">￥{{item.sell_price}}</span>
					</text>
					<view class="name">
						{{item.title}}
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return{
				swipers:[],
				shop:[]
			}
		},
		onLoad() {
			this.getSwiperData()
		},
		methods:{
			async getSwiperData(){
				// uni.request({
				// 	url:'http://localhost:8082/api/getlunbo',
				// 	method:"GET",
				// 	success: res=> {
				// 		console.log(res);
				// 	}
				// })
				const res = await this.$myRequest({
					url:'/api/getlunbo'
				})
				console.log(res)
				res.data.message.forEach(i=>{
					if(i.img=='http://www.itcast.cn/subject/webzly/images/1.2.jpg'){
						i.img='http://www.itcast.cn/subject/webzly/images/2.jpg'
					}
					this.swipers.push(i)
				})
				
				const shop=await this.$myRequest({
					url:'/api/getgoods?pageindex=1'
				})
				console.log(shop)
				shop.data.message.forEach(i=>{
					if(i.img_url=='http://demo.dtcms.net/upload/201504/20/thumb_201504200242250674.jpg'){
						i.img_url='https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1605854069097&di=34b7f282ddbc2737f03a556724c88c2d&imgtype=0&src=http%3A%2F%2F4.pic.paopaoche.net%2Fthumb%2Fup%2F2015-7%2F14365368617328916_600_0.jpg'
					}
					this.shop.push(i)
				})
				this.shop=shop.data.message
			}
		}
	}
</script>

<style lang="scss">
	.home{
		swiper{
			width: 750rpx;
			height: 360rpx;
			image{
				width: 100%;
				height: 100%;
			}
		}
	}
	.nav{
		display: flex;
		.nav_item{
			text-align: center;
			flex: 1;
			width: 25%;
			.iconfont{
				width: 120rpx;
				height: 120rpx;
				line-height: 120rpx;
				background-color: $color;
				border-radius: 50%;
				margin: 10px auto;
				color: #fff;
				font-size: 25px;
				
			}
			.icon-tupian{
				font-size: 20px;
			}
			text{
				font-size: 15px;
			}
		}
	}
	.shop{
		background-color: #eee;
		padding-top: 6rpx;
		.title{
			text-align: center;
			color: $color;
			background-color: #fff;
			padding: 12px;
			font-size: 20px;
			letter-spacing: 40rpx;
			font-weight: 500;
		}
		.shop_list{
			display: flex;
			justify-content: space-between;
			flex-wrap: wrap-reverse;
			padding: 10rpx 15rpx 0;
			.shop_item{
				background-color: #fff;
				width: 355rpx;
				image{
					width: 100%;
				}
				.price{
					margin: 15rpx 0;
					.new_price{
						color: $color;
					}
					.old_price{
						text-decoration: line-through;
						color: #999;
						font-size: 13px;
						margin-left: 10px;
					}
				}
				.name{
					font-size: 14px;
					margin-bottom: 15rpx;
				}
			}
		}
	}
</style>
