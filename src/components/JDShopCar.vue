<template>
  <div id="Shopcar">
    <div class="row">
            <div class="col-lg-10 col-lg-offset-1">
                <div class="page-header text-center">
                    <h1>购物车</h1>
                </div>
               <table class="table  table-bordered ">
                    <tr >
                        <td>
                            <input type="checkbox" style="width:20px;height:20px;" :checked="IsSelectALL" @click="selectAllShopList(IsSelectALL)">
                            <label >全选</label>
                        </td>
                        <td></td>
                        <td>名称</td>
                        <td>价格</td>
                        <td>数量</td>
                        <td>总价</td>
                        <td>操作</td>
                    </tr>
                     <tr v-for="(item,index) in ShopCarList" v-bind:key="item.shopId">
                        <td>
                            <input type="checkbox" style="width:20px;height:20px;" :checked="item.checked" @click=" getShopListPrice(item)">
                        </td>
                        <td>
                          <img :src="item.shopImage" width="80" alt="" />
                        </td>
                        <td class="text-left">{{ item.shopName}}</td>
                        <td>{{item.shopPrice | moneyFormat(item.shopPrice)}}</td>
                        <td class="col-xs-2">
                            <div class="input-group">
                                <div class="input-group-btn">
                                    <button type="button" class="btn btn-default" @click="CutShopCount(item); getAllShopCarMoney()">-</button>
                                </div>
                                <div class="input-group-input">
                                    <input type="text" v-model="item.shopNumber" value="" class="form-control ">
                                </div>
                                <div class="input-group-btn">
                                    <button type="button" class="btn btn-default" @click="AddShopCount(item); getAllShopCarMoney()">+</button>
                                </div>
                            </div>
                        </td>
                        <td>
                            {{item.shopPrice * item.shopNumber | moneyFormat(item.shopPrice * item.shopNumber)}}
                        </td>
                        <td>
                            <button class="btn btn-danger" @click="RemoveShopList(index)">删除</button>
                        </td>
                    </tr>
                   
                     <tr>
                        <td >
                            <input type="checkbox"  style="width:20px;height:20px;" :checked="IsSelectALL" @click="selectAllShopList(IsSelectALL)">
                            <label >全选</label>
                        </td>
                        <td colspan="4">
                            总价：<span class="text-danger" >{{ totalMoney | moneyFormat(totalMoney)}}</span>
                        </td>
                        <td colspan="3">
                            <button class="btn btn-success col-lg-12" @click="PayCheck()">结算</button>
                        </td>
                    </tr>
               </table>
            </div>
        </div>
  </div>
</template>
<script>
export default {
  name: "Shopcar",
  data: function() {
    return {
      ShopCarList: [],
      //是否全选
      IsSelectALL:false,
      //商品总价
      totalMoney:0,
    };
  },
  filters:{
      //过滤器  金钱格式
      moneyFormat(value){
          return "￥"+ value.toFixed(2);
      }
  },
  mounted() {
    this.getShopCarData();
  },
  methods: {
      //获取数据
    getShopCarData() {
      var _this = this;
      this.$http.get("./../static/data.json").then(function(respones) {
          //console.log(respones.data.AllShops.shopList)
        _this.ShopCarList = respones.data.AllShops.shopList;
            // console.log(_this.ShopCarList);
      });
    },
    //添加商品数量
    AddShopCount(item){
        return item.shopNumber ++ ;       
    },
    //减少数量
    CutShopCount(item){
        if(item.shopNumber <=1){
            return item.shopNumber = 1;
        }
         return item.shopNumber--;
    },
    //全选商品
    selectAllShopList(flag){
        this.IsSelectALL = !flag;
        this.ShopCarList.forEach((value,index)=>{
           // console.log(value);
            if(typeof value.checked === "undefined"){
                this.$set(value,"checked",!flag);
            }
            else{
                value.checked = !flag;
            }
        })

         this.getAllShopCarMoney();
    },
    //计算商品总价
    getAllShopCarMoney(){
        let totalPrice = 0 ;
       this.ShopCarList.forEach((value,index)=>{
           if(value.checked){
               totalPrice += value.shopNumber * value.shopPrice;
           }
       })  
       this.totalMoney = totalPrice;
    },
    //获取单个商品的选中价格
    getShopListPrice(item){
        if(typeof item.checked === "undefined"){
            this.$set(item,"checked",true);
        }
        else{
            item.checked = !item.checked ;
        }
        this.getAllShopCarMoney();
    },
    //删除商品
    RemoveShopList(index){
        this.ShopCarList.splice(index,1)
        this.getAllShopCarMoney();
        
    },
    //结账弹窗通知
    PayCheck(){
        alert("您好,一共消费 ￥"+this.totalMoney +"元");
    }
  }
};
</script>

<style>
body {
  font-size: 2em;
}
.row {
  margin-left: 0px;
  margin-right: 0px;
}

.table {
  border: 1px solid #e8e8e8;
}

.table tr td {
  border: 1px solid #e8e8e8;
  margin: 100px;
  text-align: center;
  height: 100px;
}
</style>



