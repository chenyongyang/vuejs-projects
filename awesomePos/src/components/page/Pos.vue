<template>
	<div class="pos">
		<div>
	        <el-row >
	            <el-col :span='7' class="order-list">
					<el-tabs>
						<el-tab-pane label="点餐">
							<el-table :data="tableData" border style="width: 100%" >
								<el-table-column prop="goodsName" label="商品"  ></el-table-column>
								<el-table-column prop="count" label="量" width="50"></el-table-column>
								<el-table-column prop="price" label="金额" width="70"></el-table-column>
								<el-table-column label="操作" width="100" fixed="right">
									<template slot-scope="scope">
										<el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
										<el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
									</template>
								</el-table-column>
							</el-table>
							<div class="totalDiv">
								<small>数量</small>：{{ totalCount }} &nbsp;&nbsp;&nbsp;&nbsp; <small>金额</small>：{{ totalMoney }}元
							</div>
							<div class="div-btn">
								<el-button type="warning" >挂单</el-button>
								<el-button type="danger" @click="delAllGoods">删除</el-button>
								<el-button type="success" @click="checkout">结账</el-button>
							</div>
						</el-tab-pane>
						<el-tab-pane label="挂单">
							挂单
						</el-tab-pane>
						<el-tab-pane label="外卖">
							外卖
						</el-tab-pane>
					</el-tabs>
	            </el-col>
	            <!--商品展示-->
	            <el-col :span="17" class="order-list">
					<div class="often-goods">
						<div class="title">常用商品</div>
						<div class="often-goods-list">
							<ul>
								<li v-for="goods in oftenGoods" @click="addOrderList(goods)">
									<span>{{ goods.goodsName }}</span>
									<span class="o-price">￥{{ goods.price }}</span>
								</li>
							</ul>
							<div class="clear"></div>
						</div>
					</div>
					<div class="goods-type">
					    <el-tabs>
					        <el-tab-pane label="汉堡">
					            <ul class='cookList'>
								    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
									    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
									    <span class="foodName">{{goods.goodsName}}</span>
									    <span class="foodPrice">￥{{goods.price}}元</span>
									</li>
								</ul>
					        </el-tab-pane>
					        <el-tab-pane label="小食">
					            <ul class='cookList'>
								     <li v-for="goods in type1Goods" @click="addOrderList(goods)">
								         <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
								         <span class="foodName">{{goods.goodsName}}</span>
								         <span class="foodPrice">￥{{goods.price}}元</span>
								      </li>
								</ul>
					        </el-tab-pane>
					        <el-tab-pane label="饮料">
					            <ul class='cookList'>
								     <li v-for="goods in type2Goods" @click="addOrderList(goods)">
								         <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
								         <span class="foodName">{{goods.goodsName}}</span>
								         <span class="foodPrice">￥{{goods.price}}元</span>
								      </li>
								</ul>					            
					        </el-tab-pane>
					        <el-tab-pane label="套餐">
					            <ul class='cookList'>
								     <li v-for="goods in type3Goods" @click="addOrderList(goods)">
								         <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
								         <span class="foodName">{{goods.goodsName}}</span>
								         <span class="foodPrice">￥{{goods.price}}元</span>
								      </li>
								</ul>				
					        </el-tab-pane>
					    </el-tabs>
					</div>
	            </el-col>
	        </el-row>
	    </div>
	</div>
</template>

<script type="text/javascript">
	import axios from 'axios'
	export default{
		name:'Pos',
		created(){
			axios.get('http://jspang.com/DemoApi/oftenGoods.php')
			.then(response=>{
				console.log(response);
				this.oftenGoods = response.data;
			})
			.catch(error=>{
				console.log(error);
				alert('网络错误，不能访问');
			})

			axios.get('http://jspang.com/DemoApi/typeGoods.php')
			.then(response=>{
				console.log(response);
				//this.oftenGoods=response.data;
				this.type0Goods=response.data[0];
				this.type1Goods=response.data[1];
				this.type2Goods=response.data[2];
				this.type3Goods=response.data[3];
			})
			.catch(error=>{
				console.log(error);
				alert('网络错误，不能访问');
			})
		},
		mounted:function(){
			var orderHeight=document.body.clientHeight;
			var orderList = document.getElementsByClassName("order-list")
			for (var i = 0; i < orderList.length; i++) {
				orderList[i].style.height=orderHeight+'px';
			}
		},
		data(){
			return {
				tableData:[
					// {
					// 	goodsId:1,
					// 	goodsName: '可口可乐',
					// 	price: 8,
					// 	count:1
					// }, 
					// {
					// 	goodsId:2,
					// 	goodsName: '香辣鸡腿堡',
					// 	price: 15,
					// 	count:1
					// }, 
					// {
					// 	goodsId:3,
					// 	goodsName: '爱心薯条',
					// 	price: 8,
					// 	count:1
					// }, 
					// {
					// 	goodsId:4,
					// 	goodsName: '甜筒',
					// 	price: 8,
					// 	count:1
					// }
				],
				oftenGoods:[
		          // {
		          //     goodsId:1,
		          //     goodsName:'香辣鸡腿堡',
		          //     price:18
		          // }, {
		          //     goodsId:2,
		          //     goodsName:'田园鸡腿堡',
		          //     price:15
		          // }, {
		          //     goodsId:3,
		          //     goodsName:'和风汉堡',
		          //     price:15
		          // }, {
		          //     goodsId:4,
		          //     goodsName:'快乐全家桶',
		          //     price:80
		          // }, {
		          //     goodsId:5,
		          //     goodsName:'脆皮炸鸡腿',
		          //     price:10
		          // }, {
		          //     goodsId:6,
		          //     goodsName:'魔法鸡块',
		          //     price:20
		          // }, {
		          //     goodsId:7,
		          //     goodsName:'可乐大杯',
		          //     price:10
		          // }, {
		          //     goodsId:8,
		          //     goodsName:'雪顶咖啡',
		          //     price:18
		          // }, {
		          //     goodsId:9,
		          //     goodsName:'大块鸡米花',
		          //     price:15
		          // }, {
		          //     goodsId:20,
		          //     goodsName:'香脆鸡柳',
		          //     price:17
		          // }
      			],
      			type0Goods:[
		          // {
		          //     goodsId:1,
		          //     goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
		          //     goodsName:'香辣鸡腿堡',
		          //     price:18
		          // }, {
		          //     goodsId:2,
		          //     goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
		          //     goodsName:'田园鸡腿堡',
		          //     price:15
		          // }, {
		          //     goodsId:3,
		          //     goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
		          //     goodsName:'和风汉堡',
		          //     price:15
		          // }, {
		          //     goodsId:4,
		          //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
		          //     goodsName:'快乐全家桶',
		          //     price:80
		          // }, {
		          //     goodsId:5,
		          //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
		          //     goodsName:'脆皮炸鸡腿',
		          //     price:10
		          // }, {
		          //     goodsId:6,
		          //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
		          //     goodsName:'魔法鸡块',
		          //     price:20
		          // }, {
		          //     goodsId:7,
		          //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
		          //     goodsName:'可乐大杯',
		          //     price:10
		          // }, {
		          //     goodsId:8,
		          //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
		          //     goodsName:'雪顶咖啡',
		          //     price:18
		          // }, {
		          //     goodsId:9,
		          //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
		          //     goodsName:'大块鸡米花',
		          //     price:15
		          // }, {
		          //     goodsId:20,
		          //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
		          //     goodsName:'香脆鸡柳',
		          //     price:17
		          // }          
		      	],
		      	type1Goods:[],
		      	type2Goods:[],
		      	type3Goods:[],
		      	totalCount:0,
		      	totalMoney:0
			}
		},
		methods:{
	      	//添加订单列表的方法
	      	addOrderList(goods){
	      		//console.log(goods)
	            this.totalCount=0; //汇总数量清0
	            this.totalMoney=0;
	            let isHave=false;
	            //console.log(goods.goodsId)
	            //判断是否这个商品已经存在于订单列表
	            for (let i=0; i<this.tableData.length;i++){

	                //console.log(this.tableData[i].goodsId);
	                if(this.tableData[i].goodsId==goods.goodsId){
	                    isHave=true; //存在
	                    //console.log(i)
	                }
	            }
	            //根据isHave的值判断订单列表中是否已经有此商品
	            if(isHave)
	            {
	                //存在就进行数量添加
	                 let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
	                 arr[0].count++;
	            }
	            else
	            {
	                //不存在就推入数组
	                let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
	                this.tableData.push(newGoods);

	            }

	            // //进行数量和价格的汇总计算
	            this.getAllMoney();
	           
	      	},
	      	delSingleGoods(goods){
		        //console.log(goods);
		        this.tableData=this.tableData.filter(o => o.goodsId !=goods.goodsId);
		        this.getAllMoney();
		    },
		    //把数量和金额的汇总计算单独为一个方法，提高复用性
		    getAllMoney(){
			    this.totalCount=0;
			    this.totalMoney=0;
			    if(this.tableData){
			        this.tableData.forEach((element) => {
				        this.totalCount+=element.count;
				        this.totalMoney=this.totalMoney+(element.price*element.count);   
				    });
			    }
			},
			//结账
			checkout(){
				if(this.totalCount != 0){
					this.tableData = [];
					this.totalCount = 0;
					this.totalMoney = 0;
					this.$message({
						message:'订单成功！',
						type:'success'
					});
				}else{
					this.$message.error('不能空结！');
				}
			},
			//全部删除，直接把数组清空
		    delAllGoods() {
	            this.tableData = [];
	            this.totalCount = 0;
	            this.totalMoney = 0;
	        },
  		}
	}
</script>


<style scoped>
	.title{
       height: 20px;
       border-bottom:1px solid #D3DCE6;
       background-color: #F9FAFC;
       padding:10px;
   }
   .often-goods-list ul li{
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:5px;
      background-color:#fff;
      cursor: pointer;
   }
   .clear{
   	clear: both;
   }
  .o-price{
      color:#58B7FF; 
   }
   .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auto;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
	   cursor: pointer;
   }
   .cookList li span{
       	
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 16px;
       padding-left: 10px;
       color:brown;

   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .goods-type{
		height: auto;
		overflow: hidden;
		border-top: 1px solid #D3DCE6;
   }
   .totalDiv{
		background-color: #FFF;
		padding: 10px;
		border-bottom: 1px solid #D3DCE6;
   }
</style>