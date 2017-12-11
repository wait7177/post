<template>
    <div class="pos">
      <el-row>
        <el-col :span="7" id="posorder">
          <el-tabs>
            <el-tab-pane label="点餐">
              <template>
                <el-table style="width: 100%" max-height="250" border :data="tableData">
                  <el-table-column label="商品名称"  prop="goodsName"></el-table-column>
                  <el-table-column label="数量" width="60"  prop="count"></el-table-column>
                  <el-table-column label="金钱" width="70"  prop="price"></el-table-column>
                  <el-table-column label="操作"width="100" fixed="right">
                    <template scope="scope">
                      <el-button type="text" size="small" @click="delGoods(scope.row)">删除</el-button>
                      <el-button type="text" size="small" @click="addGoods(scope.row)">增加</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </template>
              <div class="total"><span>数量:{{totalCount}}</span>金额:{{totalMoney}}</div>
              <div style="margin-top: 10px;text-align: center">
                <el-button type="primary" >挂单</el-button>
                <el-button type="success" @click="delAllGoods">删除</el-button>
                <el-button type="info" @click="checkout">结账</el-button>
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
        <el-col :span="17" >
          <div class="title">常用商品</div>
          <div style="overflow: hidden">
            <ul class="ofentgoods">
              <li v-for="goods in oftenGoods" class="ofentgoodslist" @click="addGoods(goods)"><span>{{goods.goodsName}}</span><span class="goodsmoneylist">￥{{goods.price}}元</span></li>
            </ul>
          </div>
          <div >
            <template>
              <el-tabs >
                <el-tab-pane label="汉堡" name="first">
                  <div class="hamblist">
                    <ul>
                      <li v-for="goods in type0Goods" @click="addGoods(goods)">
                        <span class="hambimg"><img :src="goods.goodsImg" alt=""></span>
                        <span class="hambname">{{goods.goodsName}}</span>
                        <span class="hambprice">￥{{goods.price}}元</span>
                      </li>
                    </ul>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="小食" name="second"><div class="hamblist">
                  <ul>
                    <li v-for="goods in type1Goods" @click="addGoods(goods)">
                      <span class="hambimg"><img :src="goods.goodsImg" alt=""></span>
                      <span class="hambname">{{goods.goodsName}}</span>
                      <span class="hambprice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
                </el-tab-pane>
                <el-tab-pane label="饮料" name="third"><div class="hamblist">
                  <ul>
                    <li v-for="goods in type2Goods" @click="addGoods(goods)">
                      <span class="hambimg"><img :src="goods.goodsImg" alt=""></span>
                      <span class="hambname">{{goods.goodsName}}</span>
                      <span class="hambprice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div></el-tab-pane>
                <el-tab-pane label="套餐" name="fourth"><div class="hamblist">
                  <ul>
                    <li v-for="goods in type3Goods" @click="addGoods(goods)">
                      <span class="hambimg"><img :src="goods.goodsImg" alt=""></span>
                      <span class="hambname">{{goods.goodsName}}</span>
                      <span class="hambprice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div></el-tab-pane>
              </el-tabs>
            </template>
          </div>
        </el-col>
      </el-row>
    </div>
</template>

<script>
  import  axios from 'axios'
    export default {
        name: 'pos',
    data(){
          return{
            tableData:[],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            totalCount:0,
            totalMoney:0
          }
    }
    ,created:function () {
        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
          .then(response=>{
            console.log(response)
            this.oftenGoods=response.data
          })
          .catch(error=>{
            alert('老铁错误了')
          })

        axios.get('http://jspang.com/DemoApi/typeGoods.php')
          .then(response=>{
            console.log(response)
            this.type0Goods=response.data[0];
            this.type1Goods=response.data[1];
            this.type2Goods=response.data[2];
            this.type3Goods=response.data[3];
          })
          .catch(error=>{
            alert('老铁错误了')
          })

      },
        mounted:function () {
          var heightorder = document.body.clientHeight
          document.getElementById('posorder').style.height=heightorder+'px'
        }
        ,methods:{
      addGoods(goods){
        let isselect = false;
        for (let i=0;i<this.tableData.length;i++){
          if (this.tableData[i].goodsId == goods.goodsId){
              isselect = true;
              this.tableData[i].count++
          }
        }
        if (isselect){
//          let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
//          arr[0].count++;
        }else {
          let newgoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1}
          this.tableData.push(newgoods)
        }
        this.newtotalCount()
      },
        delGoods(goods){
          this.tableData=this.tableData.filter(o =>o.goodsId != goods.goodsId)
          this.newtotalCount()
        },
        newtotalCount(){
          this.totalCount = 0
          this.totalMoney = 0
          for (let i=0;i<this.tableData.length;i++){
            this.totalCount +=this.tableData[i].count
            this.totalMoney += (this.tableData[i].price*this.tableData[i].count)
          }
        },
        delAllGoods(){
          this.tableData=[]
          this.totalCount = 0
          this.totalMoney = 0
        },
        checkout(){
          if (this.totalCount != 0){
            this.tableData=[]
            this.totalCount = 0
            this.totalMoney = 0
            this.$message({
              message:'结账成功！',
              type:'success'
            })
          }else {
            this.$message.error('不能空结')
          }
        }
      }
    }
</script>
<style scoped>
  .pos{
    width: 100%;
  }
#posorder{
  background-color: white;
  height: 100%;
  border: 1px solid #D3DCE6;
}
  .title{
    height: 19px;
    border-bottom: 1px solid #D3DCE6;
    width: 100%;
    padding: 10px;
  }
  .goodsmoneylist{
    color: darkcyan;
  }
  .ofentgoodslist{
    padding: 10px;
    background-color: white;
    margin: 5px;
    border: 1px solid salmon;
    float: left;
  }
  .ofentgoods{
    list-style: none;
  }
  .hamblist{
    overflow: hidden;
  }
  .hamblist li{
    list-style: none;
    width: 22%;
    overflow: hidden;
    float: left;
    /*height: auto;*/
    border: 1px solid salmon;
    margin: 2px;
  }
  .hamblist img{
    width: 100%;
    float: left;
  }
  .hamblist span{
   float: left;
  }
  .hambname{
    padding: 10px;
  }
  .hambprice{
    padding: 10px;
    display: block;
  }
  .hambimg{
    width: 40%;
  }
  .total{
    text-align: center;
    padding: 15px;
  }
  .total span{
    margin: 0 20px;
  }
  li{
    cursor: pointer;
  }
</style>
