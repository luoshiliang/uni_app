<template>
 <view class="scroll-view-container">
      <!-- 左侧的滚动视图区域 -->
      <scroll-view class="left-scroll-view" scroll-y :style="{height: wh + 'px'}">
  <block v-for="(item, i) in cateList" :key="i">
    <view :class="['left-scroll-view-item', i === active ? 'active' : '']" @click="activeChanged(i)">{{item.cat_name}}</view>
  </block>
      </scroll-view>
      <!-- 右侧的滚动视图区域 -->
<<<<<<< HEAD
      <scroll-view class="right-scroll-view" scroll-y :style="{height: wh + 'px'}">
        <view class="left-scroll-view-item">zzz</view>
        <view class="left-scroll-view-item">zzz</view>
        <view class="left-scroll-view-item">zzz</view>
        <view class="left-scroll-view-item">zzz</view>
        <view class="left-scroll-view-item">多复制一些节点，演示纵向滚动效果</view>
=======
      <scroll-view class="right-scroll-view" scroll-y :style="{height: wh + 'px'}" :scroll-top="scrollTop">
		  <view class="cate-lv2" v-for="(item2,index) in cateLevel2" :key="index">
			  <view class="cate-title">/{{item2.cat_name}}/</view>
			  <view class="cate-lv3-list">
				  <view class="cate-lv3-item" v-for="(item3,index3) in item2.children" :key="index3" @click="gotoGoodsList(item3)">
				  		<image :src="item3.cat_icon.replace('api-ugo-dev','api-ugo-web')"></image>
						<text>{{item3.cat_name}}</text>
				  </view>
			  </view>
		  </view>
>>>>>>> cate
      </scroll-view>
    </view>
  </view>
</template>

<script>
	export default {
		data() {
			return {
		    // 窗口的可用高度 = 屏幕高度 - navigationBar高度 - tabBar 高度
			wh: 0,
			//分类数据列表
			cateList:[],
			//选中项给下划线
			active:0,
		    // 二级分类列表
<<<<<<< HEAD
			cateLevel2: []
=======
			cateLevel2: [],
			scrollTop:0
>>>>>>> cate
			};
		},    
	onLoad() {
      // 获取当前系统的信息
      const sysInfo = uni.getSystemInfoSync()
      // 为 wh 窗口可用高度动态赋值
      this.wh = sysInfo.windowHeight
	  this.getCateList()
    },
	methods:{
		async getCateList(){
			    // 发起请求
			    const { data: res } = await uni.$http.get('/api/public/v1/categories')
			    // 判断是否获取失败
			    if (res.meta.status !== 200) return uni.$showMsg()
			    // 转存数据
			    this.cateList = res.message
				//因为默认选中项为第0项所以默认显示第0项的数据 
				this.cateLevel2 = res.message[0].children
		},
		activeChanged(i){
			//将选中项的索引赋值给active
			this.active = i
			
			//选中哪一项就将哪一项的子项赋值给二级分类
			this.cateLevel2 = this.cateList[i].children
<<<<<<< HEAD
=======
			
			this.scrollTop = this.scrollTop === 0 ? 1 : 0
		},
		//跳转页面
		gotoGoodsList(item){
			uni.navigateTo({
				url:'/subpkg/goods_list/goods_list?cid=' + item.cat_id
			})
>>>>>>> cate
		}
	}
	}
</script>

<style lang="scss">
.scroll-view-container {
  display: flex;
<<<<<<< HEAD

  .left-scroll-view {
    width: 120px;

    .left-scroll-view-item {
      line-height: 60px;
      background-color: #f7f7f7;
      text-align: center;
      font-size: 12px;

      // 激活项的样式
      &.active {
        background-color: #ffffff;
        position: relative;
		
		        // 渲染激活项左侧的红色指示边线
		        &::before {
		          content: ' ';
		          display: block;
		          width: 3px;
		          height: 30px;
		          background-color: #c00000;
		          position: absolute;
		          left: 0;
		          top: 50%;
		          transform: translateY(-50%);
		        }
		      }
		    }
		  }
		}
=======
>>>>>>> cate

  .left-scroll-view {
    width: 120px;

    .left-scroll-view-item {
      line-height: 60px;
      background-color: #f7f7f7;
      text-align: center;
      font-size: 12px;

      // 激活项的样式
      &.active {
        background-color: #ffffff;
        position: relative;
		
		        // 渲染激活项左侧的红色指示边线
		        &::before {
		          content: ' ';
		          display: block;
		          width: 3px;
		          height: 30px;
		          background-color: #c00000;
		          position: absolute;
		          left: 0;
		          top: 50%;
		          transform: translateY(-50%);
		        }
		      }
		    }
		  }
		}
.cate-title{
	font-size: 12px;
	font-weight: bold;
	text-align: center;
	padding: 15px 0;
}
.cate-lv3-list{
	display: flex;
	flex-wrap: wrap;
	.cate-lv3-item{
		width: 33%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 120rpx;
			height: 120rpx;
			
		}
		text{
			
		}
	}
}
</style>
