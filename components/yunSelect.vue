<template>
	<view>
		<view class="green" @click.stop="Click">
			<text>{{ items[nowSelect].name }}</text>
			<text class="ux-ykt-icon-right-arrow chooseBtn"
				   :style="{transform:show ? 'rotate(270deg)' : ''}"></text>
		</view>
		<view class="conditionBar-bottom" 
			 :style="{opacity: realShow ? 1 : '' }" 
			 v-show="vShow"  
			 @click="Click">  
			<!-- <yun-select :items="selectItems"  ref="select" @func="requestNew('selectItems','select')"></yun-select> -->
			<block v-for="item in items" :key="item.id">
				<view class="select-item" 
					  @click.stop="selectIt(item)"
					  :class="nowSelect === item.id ? 'item-active':''">
					  
				{{ item.name }}
			
				</view>
			</block>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				nowSelect:0 ,
				delayShow:false,
			};
		},
		computed:{
			realShow() {
				return this.show && this.delayShow
			},
			vShow(){
				return this.show || ( !this.show && this.delayShow )
			}
		},
		props: {
			items: {
			   type:Array,
			   default: null
			},
			show: {
				type:Boolean,
				default: true
			},
			func: {
				type:Function,
				default: null
			}
		},
		methods: {
			selectIt(item) {
				this.nowSelect = item.id;
			},
			Click() {
				if(this.show === this.delayShow)
					this.$emit('yunClick');
			}
		},
		watch: {
			'nowSelect'() {
				this.$emit('func')
			},
			'show'(newVal) {
				clearTimeout(d)
				let delay =  newVal ? 0 : 600;
				let d = setTimeout(() => {
					this.delayShow = newVal;
				}, delay)
			}
		}
	}
</script>

<style lang="scss">
	$green: #36b378;
	
	.green {
		color: $green;
	}
	.chooseBtn {
		display: inline-block;
		font-size: 12px;
		margin-left: 15rpx;
		transform: rotate(90deg);
		transition: all .5s;
	}
	.conditionBar-bottom {
		width: 100vw;
		height: 100vh;
		position: absolute;
		top:87rpx;
		left:0;
		background-color:rgba(0,0,0,0.5);
		opacity: 0;
		transition: opacity .5s;
		
		.select-item {
			height: 85rpx;
			text-align: center;
			line-height: 85rpx;
			background: #fff;
			font-size: 15px;
			color: #3c4a55;
		}	
		
		.item-active {
			background: #f6f6f6;
			color: $green;
		}	
	}
	
</style>
