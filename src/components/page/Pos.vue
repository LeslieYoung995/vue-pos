<template>
    <div class="pos">
      <el-row>
        <el-col :span="7" class="pos_order" id="goods_order">
          <el-tabs type="card">
            <el-tab-pane label="点 餐">
              <el-table :data="tableData" border style="width: 100%">
                <el-table-column prop="goodsName" label="商品名称" width="110">
                </el-table-column>
                <el-table-column prop="count" label="数量" width="70">
                </el-table-column>
                <el-table-column prop="price" label="金额" width="70">
                </el-table-column>
                <el-table-column label="操作" fixed="right" width="100">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="delAGoods(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addorderlist(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div>
                <small>数量：</small>{{many}} &nbsp;&nbsp;&nbsp;&nbsp;<small>金额：</small>{{pay}}元
              </div>
              <div class="btn_box">
                <el-button type="info">挂单</el-button>
                <el-button type="danger" @click="del()">删除</el-button>
                <el-button type="success">结账</el-button>
              </div>

            </el-tab-pane>
            <el-tab-pane label="挂 单">2</el-tab-pane>
            <el-tab-pane label="外 卖">3</el-tab-pane>
          </el-tabs>
        </el-col>

        <el-col :span="16" class="goods">
          <div class="often_goods">
            <div class="goods_title">热门商品</div>
            <div class="often_goods_list">
              <ul>
                <li v-for="goods in oftenGoods" @click="addorderlist(goods)">
                  <span>{{goods.goodsName}}</span>
                  <span class="goods_price">￥{{goods.price}}元</span>
                </li>
              </ul>
            </div>
          </div>

          <div class="goods_type">
            <el-tabs type="card">
              <el-tab-pane label="主食" >
                <div>
                  <ul class="food_list">
                    <li v-for="goods in type0Goods" @click="addorderlist(goods)">
                      <span class="food_img"><img :src="goods.goodsImg" alt=""></span>
                      <span class="food_name">{{goods.goodsName}}</span>
                      <span class="food_price">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>

              <el-tab-pane label="小食" >
                <div>
                  <ul class="food_list">
                    <li v-for="goods in type1Goods" @click="addorderlist(goods)">
                      <span class="food_img"><img :src="goods.goodsImg" alt=""></span>
                      <span class="food_name">{{goods.goodsName}}</span>
                      <span class="food_price">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>

              <el-tab-pane label="饮品" >
                <div>
                  <ul class="food_list">
                    <li v-for="goods in type2Goods" @click="addorderlist(goods)">
                      <span class="food_img"><img :src="goods.goodsImg" alt=""></span>
                      <span class="food_name">{{goods.goodsName}}</span>
                      <span class="food_price">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>

              <el-tab-pane label="套餐" >
                <div>
                  <ul class="food_list">
                    <li v-for="goods in type3Goods" @click="addorderlist(goods)">
                      <span class="food_img"><img :src="goods.goodsImg" alt=""></span>
                      <span class="food_name">{{goods.goodsName}}</span>
                      <span class="food_price">￥{{goods.price}}元</span>
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
    import axios from 'axios';
    export default {
        name: "pos",
        data() {
          return {
            tableData: [],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            pay:0,
            many:0
          };
        },
        created:function(){
          axios.get("http://jspang.com/DemoApi/oftenGoods.php").then(reponse=>{
            this.oftenGoods=reponse.data;
          }).catch(err=>{
            alert("网络错误，获取数据失败...");
          });
          axios.get('http://jspang.com/DemoApi/typeGoods.php')
            .then(response=>{
              console.log(response);
              this.type0Goods=response.data[0];
              this.type1Goods=response.data[1];
              this.type2Goods=response.data[2];
              this.type3Goods=response.data[3];
            })
            .catch(error=>{
              console.log(error);
              alert('网络错误，不能访问...');
            })
        },
        mounted:function () {
          var winHeight = document.body.clientHeight;
          document.getElementById("goods_order").style.height=winHeight+"px";
        },
        methods:{
          //商品是否存在于列表中
          addorderlist(goods){

            let hasgoods=false;
            for(let i=0;i<this.tableData.length;i++){
              if(this.tableData[i].goodsId==goods.goodsId){
                hasgoods=true;
              }
            }
            //如果存在则进行数量的增加
            if(hasgoods){
              let arr=this.tableData.filter(o=>o.goodsId==goods.goodsId);
                arr[0].count++;
              } else{
                //如果不存在则添加商品属性
                let newgoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
                this.tableData.push(newgoods);
              }
              this.getAllMoney();
            },
          del(){  //删除所有商品
            this.tableData=[];
            this.many=0;
            this.pay=0;
          },
          getAllMoney(){ //计算金额和数量
            this.pay=0;
            this.many=0;
            this.tableData.forEach((index)=>{
              this.many+=index.count;
              this.pay+=(index.count*index.price);
            })
          },
          delAGoods(goods){ //删除一件商品
            this.tableData=this.tableData.filter(o=>o.goodsId!=goods.goodsId);
            this.getAllMoney();
            },
        }
    }
</script>

<style scoped>
  .pos_order{
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
  }
  .btn_box{
    margin-top: 15px;
  }
  .goods_title{
    font-family: "Microsoft JhengHei";
    height:20px;
    padding:10px;
    background-color: #F9FAFC;
    border-bottom:1px solid #D3DCE6;
    text-align: left;
  }
  .often_goods_list ul li{
    list-style: none;
    float: left;
    border:1px solid #E5E9F2;
    padding: 10px;
    margin:10px;
    background-color: #FFF;
    cursor: pointer;
  }
  .goods_price{
    color:#58B7FF;
  }
  .goods_type{
    clear: both;
    padding-top:15px;
  }
  .food_list li{
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
  .food_list li span{

    display: block;
    float:left;
  }
  .food_img{
    display: block;
    float:left;
    width: 40%;
    height:83px;
  }
  .food_img img{
    width: 100%;
    height: 100%;
  }
  .food_name{
    font-size: 18px;
    padding-left: 10px;
    color:brown;

  }
  .food_price{
    font-size: 16px;
    padding-left: 10px;
    padding-top:10px;
  }
</style>
