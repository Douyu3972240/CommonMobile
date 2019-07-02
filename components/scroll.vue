<template>
	<view>
		<view class="content">
			<view class="left" ref="left" :style="scrollHeightPX" >
				<view>
					<view ref="leftList" v-on:click="selectMenu(p)" :y="y" :class="(y >= i.top && y < i.bottom) || (p == 0 && y ==0) ?'leftListItem active ':'leftListItem'"
					 v-for="(i,p) in left" :key="p">{{i.name}}</view>
				</view>
			</view>
			<view class="right" ref="right"  :style="scrollHeightPX">
				<view class="rightListBox">
					<view ref="rightList" v-for="(i,p) in right" :key="p" class="rightList">
						<view style="display: block;width:100%;">{{i.name}}</view>
						<view v-for="(k,q) in i.list" :key="q">
							<view style="display: block;width:100%;">{{k}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import BScroll from 'better-scroll';
	export default {
		data() {
			return {
				scrollHeight:600,	//這個是滑动区域的高度
				scrollHeightPX:"",	//想高度写成height:xxxpx;
				y: 0,
				left: [{
					"name": "项目1"
				}, {
					"name": "项目2"
				}, {
					"name": "项目3"
				}, {
					"name": "项目4"
				}, {
					"name": "项目5"
				}, {
					"name": "项目6"
				}, {
					"name": "项目7"
				}, {
					"name": "项目8"
				}, { 
					"name": "项目9"
				}, {
					"name": "项目10"
				}],
				right: [{
					"name": "项目1",
					"list": [1, 2, 3]
				}, {
					"name": "项目2",
					"list": [1, 2, 3, 4, 5, 6, 7, 8]
				}, {
					"name": "项目3",
					"list": [1]
				}, {
					"name": "项目4",
					"list": [1, 2, 3, 4, 5]
				}, {
					"name": "项目5",
					"list": [1, 2, 3, 4, 5, 6]
				}, {
					"name": "项目6",
					"list": [1, 2, 3, 4, 5, 6]
				}, {
					"name": "项目7",
					"list": [1, 2, 3, 4, 5, 6]
				}, {
					"name": "项目8",
					"list": [1, 2, 3, 4, 5, 6]
				}, {
					"name": "项目9",
					"list": [1, 2, 3, 4, 5, 6]
				}, {
					"name": "项目10",
					"list": [1, 2, 3, 4, 5, 6,7,9,10,11,12]
				}]
			}
		},
		created() {
			console.clear()
			console.log("created")
			

		},
		computed: {

		},
		mounted() {
			
			console.log("mounted")
			
			this.y = 0
			this.scrollHeightPX = "height:"+ this.scrollHeight.toString()+"px"
			this.$nextTick(() => {
				
				// console.log(this.$refs.right.$el)
				
				this.int()
				//计算出top，bottom，然后通过y，top，bottom来判断用户滑动到了哪个栏目了。且高亮显示
				for (let i = 0; i < this.left.length; i++) {
					this.left[i].height = Math.abs(this.$refs.leftList[i].$el.offsetTop)+Math.abs(this.$refs.leftList[i].$el.offsetHeight)
					this.left[i].top = Math.abs(this.$refs.rightList[i].$el.offsetTop)
					this.left[i].bottom = Math.abs(this.$refs.rightList[i].$el.offsetTop) + Math.abs(this.$refs.rightList[i].$el.offsetHeight)
				}
				console.log(this.left)
			})


			
		},
		methods: {
			selectMenu(ind){
				this.iRight.scrollTo(0,-Math.abs(this.left[ind].top),300);
			},
			int() {
				this.iLeft = new BScroll('.left', {
					scrollY: true,
					click: true,
					probeType: 3
				})
				this.iRight = new BScroll('.right', {
					scrollY: true,
					click: true,
					probeType: 3
				})
				this.iRight.on('scroll', (pos) => {
					this.y = Math.abs(pos.y)
					//下面这个循环用于右边滑动时左边自动定位显示
					for(let i = 0; i < this.left.length; i++){
						if(this.y >= this.left[i].top && this.y < this.left[i].bottom){
							if(this.scrollHeight <= this.left[i].height){
								this.iLeft.scrollTo(0,-this.left[i-1].height,300)
							}else if(this.scrollHeight > this.left[i].height){
								this.iLeft.scrollTo(0,-this.left[i].top,300)
							}
							
						}
					}
				})

			}
		}
	}
</script>

<style lang="less">
	.leftListItem {
		color: #333;
		transition: color .3s;
		border-bottom: 1px solid #ccc;
		font-size: 32upx;
		line-height: 500upx;
	}

	.active {
		color: red;
	}

	.content {
		width: 100%;
	}

	.rightListBox {
		width: 100%;
		// height:300px;
	}

	.left,
	.right {
		float: left;
	}

	.right {
		width: 70%;
	}

	.left {
		width: 30%;
	}

	.right,
	.left {
		// height: 600px;
		// overflow: hidden;

	}
</style>
