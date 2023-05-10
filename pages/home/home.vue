<template>
	<view>
		<!-- 轮播图 -->
		<swiper
		:indicator-dots="true"
		:autoplay="true"
		:interval="3000"
		:duration="1000"
		:circular="true"
		>
			<swiper-item 
			v-for="(swiper,index) in swiperList" 
			:key="swiper['goods_id']">
				<navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id='+swiper['goods_id']">
					<image :src="swiper['image_src']"></image>
				</navigator>
			</swiper-item>
		</swiper>
		
		<!-- 分类导航 -->
		<view class="nav-list">
			<view class="nav-item" v-for="(item,index) in navList" :key="index" @click="navClickHandler(item)">
				<image :src="item['image_src']" class="nav-img"></image>
			</view>
		</view>
		
		<!-- 楼层区域 -->
		<view class="floor-list">
		  <!-- 楼层 item 项 -->
		  <view class="floor-item" v-for="(item, i) in floorList" :key="i">
		    <!-- 楼层标题 -->
		    <image :src="item.floor_title.image_src" class="floor-title"></image>
		       <!-- 楼层图片区域 -->
		       <view class="floor-img-box">
		         <!-- 左侧大图片的盒子 -->
		         <navigator class="left-img-box" :url="item.product_list[0].url">
		           <image :src="item.product_list[0].image_src" :style="{width: item.product_list[0].image_width + 'rpx'}" mode="widthFix"></image>
		         </navigator>
		         <!-- 右侧 4 个小图片的盒子 -->
		         <view class="right-img-box">
		           <navigator class="right-img-item" v-for="(item2, i2) in item.product_list" :key="i2" v-if="i2 !== 0" :url="item2.url">
		             <image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}"></image>
		           </navigator>
		         </view>
		       </view>
		  </view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//轮播图数据
				swiperList:[],
				navList:[],
				floorList:[]
			};
		},
		onLoad(){
			this.getSwiperList()
			this.getNavList()
			this.getFloorList()
		},
		methods:{
			//轮播图数据
			async getSwiperList(){
				const { data:res } = await uni.$http.get('/api/public/v1/home/swiperdata')
				if(res.meta.status !== 200){
					return 
					uni.$showMsg()
				}
				this.swiperList = res.message
			},
			//分类数据
			async getNavList(){
				const {data:res} = await uni.$http.get('/api/public/v1/home/catitems')
				console.log(res)
				if(res.meta.status !== 200){
					return 
					uni.$showMsg()
				}
				this.navList = res.message
			},
			async getFloorList(){
				 const { data: res } = await uni.$http.get('/api/public/v1/home/floordata')
				    if (res.meta.status !== 200){
					  return
					  uni.$showMsg()	
					}
					
					  // 通过双层 forEach 循环，处理 URL 地址
					  res.message.forEach(floor => {
					    floor.product_list.forEach(prod => {
							//将url进行重新赋值   split('?')[0] 将navigator_url 利用问号分开组成数组 拿第一项 也就是query参数
					      prod.url = '/subpkg/goods_list/goods_list?' + prod.navigator_url.split('?')[1]
					    })
					  })
					  
				    this.floorList = res.message
			},
			navClickHandler(item){
				if(item.name == '分类'){
					uni.switchTab({
						url:'/pages/cate/cate'
					})
				}
			}
		}
	}
</script>

<style lang="scss">
swiper{
	height: 330rpx;
}
	
.swiper-item,
image{
	width: 100%;
}
.nav-list{
	display: flex;
	justify-content: space-around;
	 margin: 15px 0;
	.nav-img{
		width: 128rpx;
		height: 140rpx;
	}
}
.floor-title {
  height: 60rpx;
  width: 100%;
  display: flex;
}
.floor-img-box {
    display: flex;
    padding-left: 10rpx;
  }
.right-img-box {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }
</style>
