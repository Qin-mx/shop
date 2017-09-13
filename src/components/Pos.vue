<template>
	<div class="pos" v-cloak>
		<el-row id="order-list">
			<el-col :span='8'>
				<!--点餐区域-->
				<el-tabs>
					<el-tab-pane label="点餐">
						<el-table :data="tableData" border style="width: 100%">
							<el-table-column prop="goodsName" label="商品"></el-table-column>
							<el-table-column prop="count" label="数量" width="100"></el-table-column>
							<el-table-column prop="amount" label="金额" width="100"></el-table-column>
							<el-table-column fixed="right" label="操作">
								<template scope="scope">
									<el-button type="text" size="small" @click="pushType(scope.row)">增加</el-button>
									<el-button type="text" size="small" @click="removeType(scope.row)">删除</el-button>
								</template>
							</el-table-column>
						</el-table>
						<div class="num">
							<span>数量：{{totalCount}} 个</span>
							<span>总额：{{totalMoney}} 元</span>
						</div>
						<div class="btn">
							<el-button type="danger" @click="delAllList">删除</el-button>
							<el-button type="success" @click="upList">结账</el-button>
						</div>

					</el-tab-pane>

				</el-tabs>

			</el-col>
			<el-col :span='16' class="right-list">
				<!--常用商品选择-->
				<div class="often-shop">
					<h2>常用商品</h2>
					<div class="often-goods-list">
						<ul class="clex">
							<li v-for="item in oftenGoods" @click="pushType(item)"><span>{{item.goodsName}}</span><span>&yen;{{item.price}}元</span></li>
						</ul>
					</div>
				</div>
				<!--商品列表-->
				<el-tabs>
					<el-tab-pane label="汉堡">
						<ul class='cookList clex'>
							<li v-for="item in type0Goods" @click="pushType(item)">
								<span class="foodImg"><img v-bind:src="item.goodsImg" width="100%"></span>
								<span class="foodName">{{ item.goodsName }}</span>
								<span class="foodPrice">￥{{ item.price }}元</span>
							</li>
						</ul>
					</el-tab-pane>
					<el-tab-pane label="小食">
						<ul class='cookList clex'>
							<li v-for="item in type1Goods" @click="pushType(item)">
								<span class="foodImg"><img v-bind:src="item.goodsImg" width="100%"></span>
								<span class="foodName">{{ item.goodsName }}</span>
								<span class="foodPrice">￥{{ item.price }}元</span>
							</li>
						</ul>
					</el-tab-pane>
					<el-tab-pane label="饮料">
						<ul class='cookList clex'>
							<li v-for="item in type2Goods" @click="pushType(item)">
								<span class="foodImg"><img v-bind:src="item.goodsImg" width="100%"></span>
								<span class="foodName">{{ item.goodsName }}</span>
								<span class="foodPrice">￥{{ item.price }}元</span>
							</li>
						</ul>
					</el-tab-pane>
					<el-tab-pane label="套餐">
						<ul class='cookList clex'>
							<li v-for="item in type3Goods" @click="pushType(item)">
								<span class="foodImg"><img v-bind:src="item.goodsImg" width="100%"></span>
								<span class="foodName">{{ item.goodsName }}</span>
								<span class="foodPrice">￥{{ item.price }}元</span>
							</li>
						</ul>
					</el-tab-pane>

				</el-tabs>
			</el-col>
		</el-row>
	</div>
</template>

<script>
	import axios from 'axios'
	export default {
		name: 'pos',
		data() {
			return {
				//				tableData: [{
				//					goodsName: '可口可乐',
				//					amount: 8,
				//					count: 1
				//				}, {
				//
				//					goodsName: '香辣鸡腿堡',
				//					amount: 15,
				//					count: 1
				//				}, {
				//
				//					goodsName: '爱心薯条',
				//					amount: 8,
				//					count: 1
				//				}, {
				//
				//					goodsName: '甜筒',
				//					amount: 8,
				//					count: 1
				//				}],
				//				oftenGoods: [{
				//							goodsId: 1,
				//							goodsName: '香辣鸡腿堡',
				//							price: 18
				//						}, {
				//							goodsId: 2,
				//							goodsName: '田园鸡腿堡',
				//							price: 15
				//						}, {
				//							goodsId: 3,
				//							goodsName: '和风汉堡',
				//							price: 15
				//						}, {
				//							goodsId: 4,
				//							goodsName: '快乐全家桶',
				//							price: 80
				//						}, {
				//							goodsId: 5,
				//							goodsName: '脆皮炸鸡腿',
				//							price: 10
				//						}, {
				//							goodsId: 6,
				//							goodsName: '魔法鸡块',
				//							price: 20
				//						}, {
				//							goodsId: 7,
				//							goodsName: '可乐大杯',
				//							price: 10
				//						}, {
				//							goodsId: 8,
				//							goodsName: '雪顶咖啡',
				//							price: 18
				//						}, {
				//							goodsId: 9,
				//							goodsName: '大块鸡米花',
				//							price: 15
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}, {
				//							goodsId: 20,
				//							goodsName: '香脆鸡柳',
				//							price: 17
				//						}
				//
				//					],
				//				type0Goods: [{
				//						goodsId: 1,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
				//						goodsName: '香辣鸡腿堡',
				//						price: 18
				//					}, {
				//						goodsId: 2,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
				//						goodsName: '田园鸡腿堡',
				//						price: 15
				//					}, {
				//						goodsId: 3,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
				//						goodsName: '和风汉堡',
				//						price: 15
				//					}, {
				//						goodsId: 4,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
				//						goodsName: '快乐全家桶',
				//						price: 80
				//					}, {
				//						goodsId: 5,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
				//						goodsName: '脆皮炸鸡腿',
				//						price: 10
				//					}, {
				//						goodsId: 6,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
				//						goodsName: '魔法鸡块',
				//						price: 20
				//					}, {
				//						goodsId: 7,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
				//						goodsName: '可乐大杯',
				//						price: 10
				//					}, {
				//						goodsId: 8,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
				//						goodsName: '雪顶咖啡',
				//						price: 18
				//					}, {
				//						goodsId: 9,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
				//						goodsName: '大块鸡米花',
				//						price: 15
				//					}, {
				//						goodsId: 20,
				//						goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
				//						goodsName: '香脆鸡柳',
				//						price: 17
				//					}
				//
				//				],
				tableData: [],
				oftenGoods: [],
				type0Goods: [],
				type1Goods: [],
				type2Goods: [],
				type3Goods: [],
				totalCount: 0,
				totalMoney: 0,
			}
		},
		created() {
			//			通过axios获取数据 ，同jsp的接口调用
			//			获取常用商品分类
			axios.get('http://jspang.com/DemoApi/oftenGoods.php')
				.then(
					response => {
						//						console.log(response)
						this.oftenGoods = response.data
					}
				)
				.catch(error => {
					console.log(error)
					alert('加载失败' + response.status)
				})
			//			获取商品分类
			axios.get('http://jspang.com/DemoApi/typeGoods.php')
				.then((response) => {
					//					这里用箭头函数是因为普通函数的this丢失了
					this.type0Goods = response.data[0];
					this.type1Goods = response.data[1];
					this.type2Goods = response.data[2];
					this.type3Goods = response.data[3];
				})
				.catch(error => {
					alert('加载失败' + response.status)
				})
		},
		mounted() {
			var orderHeight = document.body.scrollHeight;
			document.getElementById('order-list').style.height = orderHeight + 'px';
		},
		methods: {
			getAllMoney() {
				this.totalCount = 0;
				this.totalMoney = 0;
				//				判断数据
				if(this.tableData) {
					for(var i in this.tableData) {
						this.totalCount += this.tableData[i].count;
						this.totalMoney += this.tableData[i].count * this.tableData[i].amount;
					}
				}
			},
			pushType(goods) {
				this.totalCount = 0;
				this.totalMoney = 0;
				//				判断是否存在数据
				var isHave = false; //判断是否重复
				for(var i = 0; i < this.tableData.length; i++) {
					if(this.tableData[i].goodsId == goods.goodsId) {
						isHave = true;
					}
				}
				//				进行数据判断
				if(isHave) {
					//	如果该数据存在
					//过滤掉相同的数据,数量+1
					var arr = this.tableData.filter(e => e.goodsId == goods.goodsId)
					arr[0].count++
				} else {
					//					如果不存在进行,则添加到数据中
					var newObj = {
						goodsId: goods.goodsId,
						goodsName: goods.goodsName,
						amount: goods.price,
						count: 1
					}
					this.tableData.push(newObj)
				}
				//				计算总量
				this.getAllMoney()
			},
			removeType(goods) {
				this.tableData = this.tableData.filter(e => e.goodsId != goods.goodsId)
				this.getAllMoney()
			},

			delAllList() {
				this.tableData = [];
				this.totalCount = 0;
				this.totalMoney = 0;
			},
			upList() {
				//				模拟结账
				if(this.tableData) {
					this.totalCount = 0
					this.totalMoney = 0
					this.tableData = []
									console.log('===上述方法失效，直接弹框===')
					this.$message({
						type: 'succes',
						message: '结算成功'
					})
				} else {
										console.log('上述方法失效，直接弹框')
					this.$message({
						type: 'error',
						message: '请核对信息'
					})
				}
			}
		}
	}
</script>

<style>
	html,
	body,
	#app {
		height: 100%;
		padding: 0;
		margin: 0;
	}
	
	.clex:after {
		content: '';
		display: block;
		clear: both;
	}
	
	[v-cloak] {
		display: none;
	}
	
	.title {
		height: 21px;
		border-bottom: 1px solid #D3DCE6;
		background-color: #F9FAFC;
		padding: 10px;
	}
	
	.often-goods-list ul li {
		list-style: none;
		float: left;
		border: 1px solid #E5E9F2;
		padding: 10px;
		margin: 5px;
		background-color: #fff;
		cursor: pointer;
	}
	
	.o-price {
		color: #58B7FF;
	}
	
	.cookList li {
		list-style: none;
		width: 23%;
		border: 1px solid #E5E9F2;
		cursor: pointer;
		overflow: hidden;
		background-color: #fff;
		padding: 2px;
		float: left;
		margin: 2px;
	}
	
	.cookList li span {
		display: block;
		float: left;
	}
	
	.foodImg {
		width: 40%;
	}
	
	.foodName {
		display: block;
		width: 100px;
		height: 40px;
		font-size: 18px;
		padding-left: 10px;
		color: brown;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
	}
	
	.foodPrice {
		font-size: 16px;
		padding-left: 10px;
		padding-top: 10px;
	}
	
	.btn {
		margin: 20px 0;
	}
	
	.right-list {
		border-left: 2px solid #2C3E50;
		background: #D3DCE6;
	}
	
	.num {
		text-align: center;
		padding: 10px 0;
	}
	
	.cell {
		text-align: center;
	}
</style>