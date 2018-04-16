<template>
	
<div id="pox">
	<el-row>
	  <el-col :span="7" class="pos-order" id="order-list">
	    <el-tabs type="border-card">
	     <el-tab-pane label='点餐'>
	      <el-table :data="tableData" border style="width:100%;">
	     	<el-table-column prop="goodsName" label="商品名称"></el-table-column>
	     	<el-table-column prop="count" label="数量" width="60"></el-table-column>
	     	<el-table-column prop="price" label="单价" width="60"></el-table-column>
	     	<el-table-column label="操作" width="90" fixed="right">
	     		<template slot-scope="scope">
	     		<el-button type="text" size="small" @click="delGoods(scope.row)">删除</el-button>
	     		<el-button type="text" size="small" @click="addOrderlist(scope.row)">增加</el-button>	
	     		</template>
	     	</el-table-column>
	      </el-table>
	      <div class="total">
	      	<span>数量：{{totalCount}} 个</span><span>总金额：{{totalMoney}} 元</span>
	      </div>
	      <div class="order-btn">
	      <el-button type="info" size="medium">挂单</el-button>
	      <el-button type="danger" size="medium" @click="delAll">删除</el-button>
	      <el-button type="success" size="medium" @click="checkout">结账</el-button>	   
	      </div>
	      </el-tab-pane>
	      <el-tab-pane label='挂单'>挂单</el-tab-pane>
	      <el-tab-pane label='外卖'>外卖</el-tab-pane>
	      </el-tabs>
	    
	  </el-col>
	    <el-col :span="17">
	    	<div class="often-goods">
	    	<div class="title">常用商品</div>
	    	<div class="often-goods-lis">
	    	 <ul>
	    	  <li v-for="goods in oftenGoods" @click="addOrderlist(goods)">
	    	  	<span>{{goods.goodsName}}</span>
	    	  	<span class="o-price">￥{{goods.price}}元</span>
	    	  </li>
	    	 </ul>
	    	</div>
	       </div>
	       
	       
	       
	       <div class="goods-type">
	       	<el-tabs type="border-card">
	       		<el-tab-pane label="汉堡">	
	       			<div>
	       			  <ul class='cookList'>
                       <li v-for="goods in type0goods" @click="addOrderlist(goods)">
	                   <span class="foodImg"><img :src="goods.goodsImg" width="100%"/></span>
	                   <span class="foodName">{{goods.goodsName}}</span>
	                   <span class="foodPrice">￥{{goods.price}}元</span>
                      </li>
                     </ul>
	       			</div>
	       		</el-tab-pane>
	       		<el-tab-pane label="小食">
	       			<div>
	       			  <ul class='cookList'>
                       <li v-for="goods in type1goods" @click="addOrderlist(goods)">
	                   <span class="foodImg"><img :src="goods.goodsImg" width="100%"/></span>
	                   <span class="foodName">{{goods.goodsName}}</span>
	                   <span class="foodPrice">￥{{goods.price}}元</span>
                      </li>
                     </ul>
	       			</div>
	       		</el-tab-pane>
	       		<el-tab-pane label="饮品">
	       			<div>
	       			  <ul class='cookList'>
                       <li v-for="goods in type2goods" @click="addOrderlist(goods)">
	                   <span class="foodImg"><img :src="goods.goodsImg" width="100%"/></span>
	                   <span class="foodName">{{goods.goodsName}}</span>
	                   <span class="foodPrice">￥{{goods.price}}元</span>
                      </li>
                     </ul>
	       			</div>
	       		</el-tab-pane>
	       		<el-tab-pane label="套餐">
	       			<div>
	       			  <ul class='cookList'>
                       <li v-for="goods in type3goods" @click="addOrderlist(goods)">
	                   <span class="foodImg"><img :src="goods.goodsImg" width="100%"/></span>
	                   <span class="foodName">{{goods.goodsName}}</span>
	                   <span class="foodPrice">￥{{goods.price}}元</span>
                      </li>
                     </ul>
	       			</div>
	       		</el-tab-pane>
	       	</el-tabs>
	       </div>
	    </el-col>
	</el-row>
</div>
	
</template>

<script>
	import axios from 'axios'
	export default{
		name:'pos',
		data(){
			return{
			 tableData:[],
			 oftenGoods:[],
			 type0goods:[],
			 type1goods:[],
			 type2goods:[],
			 type3goods:[],
			 totalMoney:0,
			 totalCount:0
			}
		},
		created:function(){
			axios.get('http://jspang.com/DemoApi/oftenGoods.php').then(res=>{
				this.oftenGoods=res.data;
			}).catch(error=>{
				alert('抱歉，暂时无法访问')
			})
			
			axios.get('http://jspang.com/DemoApi/typeGoods.php').then(res=>{
				this.type0goods=res.data[0];
				this.type1goods=res.data[1];
				this.type2goods=res.data[2];				
				this.type3goods=res.data[3];				
			}).catch(error=>{
				alert('抱歉，暂时无法访问')
			})
		},
		mounted:function(){
			var high=document.body.clientHeight;
			document.getElementById('order-list').style.height=high+'px';
		},
		methods:{
			addOrderlist(goods){
				 
         		  let isHave=false;
				
				for(let i=0;i<this.tableData.length;i++){
					if(this.tableData[i].goodsId==goods.goodsId){
					   isHave=true;	
					};
				};
				//根据判断编写业务逻辑
				if(isHave){
					let arr=this.tableData.filter(a=>a.goodsId==goods.goodsId)
					arr[0].count++;
				}else{
					let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
					this.tableData.push(newGoods);
				};
				
				this.gettotalMoney(goods);
			},
			delGoods(goods){
				this.tableData=this.tableData.filter((a)=>a.goodsId!=goods.goodsId)
				this.gettotalMoney(goods);
			},
			gettotalMoney(){
				 this.totalCount=0;
				 this.totalMoney=0;
				 if(this.tableData){
				 	this.tableData.forEach((item)=>{
				 		this.totalCount+=item.count;
				 		this.totalMoney=this.totalMoney+(item.count*item.price);
				 	})
				 }
			},
			delAll(){
				this.tableData=[];
				this.totalCount=0;
				this.totalMoney=0;
			},
			checkout(){
				if(this.totalMoney){
				this.tableData=[];
				this.totalCount=0;
				this.totalMoney=0;
				this.$message({
					message:'结账成功',
					type:'success'
				})
				}else{
					this.$message.error('老板，不能空结啊！')
				}
			}
		}
	}
</script>

<style scoped>
.pos{
	font-size: 14px;
}
.pos-order {
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
    height: 100%;
}
.order-btn{
	margin-top:10px;
	text-align:center;
}
.title{
	height: 20px;
	border-bottom: 1px solid gray;
	background-color: #F9FAFC;
	text-align: left;
	padding: 9px;
}
.often-goods-lis ul li{
	list-style: none;
	float: left;
	border: 1px solid lightskyblue;
	padding: 10px;
	margin: 10px;
	background-color: white;
	cursor: pointer;
}
.o-price{
	color: #58b7ff;
}
.goods-type{
	clear: both;
}
.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
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
.total{
	width:100%;
	padding: 10px;
}
.total span{
	margin-right:15px;
}	
</style>