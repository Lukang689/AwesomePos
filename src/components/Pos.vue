<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="7" id="order-list">
          <el-tabs type="border-card">
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border style="width:100%">
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="count" label="数量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column prop="operation" label="操作" width="100">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="minusFoodCount(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addFoodCount(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <el-table :data="totalData" border style="width: 100%" class="total-data">
                <el-table-column prop="totalPrice" label="总计金额(单位：元)"></el-table-column>
              </el-table>
              <el-row class="button-group">
                <el-col>
                    <el-button type="danger">删除</el-button>
                    <el-button type="warning">挂单</el-button>
                    <el-button type="success">结账</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="挂单">
              挂单
            </el-tab-pane>
            <el-tab-pane label="外卖">
              外卖
            </el-tab-pane>
          </el-tabs>
        </el-col>
        <el-col :span="17">
          <el-row class="often-goods">
            <el-col>
                <h4 class="title">顾客常点食品</h4>
                  <ul class="goods-list">
                    <li v-for="(goods,index) in oftenGoods" :key="index" @click="addOrderList(goods)">
                      <span>{{goods.goodsName}}</span>
                      <span class="goods-price">{{goods.price}}</span>
                    </li>
                  </ul>
            </el-col>
          </el-row>
          
          <el-row class="goods-type">
            <el-col>
              <el-tabs type="border-card">
              <el-tab-pane label="主食">
                <ul class="cook-list">
                  <li v-for="(goods,index) in typeGoodsStapleFood" v-bind:key="index" @click="addOrderList(goods)">
                    <div class="food-img">
                      <img :src="goods.goodsImg" alt="" width="100%">
                    </div>
                    <p class="food-name">{{goods.goodsName}}</p>
                    <p class="food-price">{{goods.price}}</p>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class="cook-list">
                  <li v-for="(goods,index) in typeGoodsCookies" v-bind:key="index" @click="addOrderList(goods)">
                    <div class="food-img">
                      <img :src="goods.goodsImg" alt="" width="100%">
                    </div>
                    <p class="food-name">{{goods.goodsName}}</p>
                    <p class="food-price">{{goods.price}}</p>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class="cook-list">
                  <li v-for="(goods,index) in typeGoodsDrinks" v-bind:key="index" @click="addOrderList(goods)">
                    <div class="food-img">
                      <img :src="goods.goodsImg" alt="" width="100%">
                    </div>
                    <p class="food-name">{{goods.goodsName}}</p>
                    <p class="food-price">{{goods.price}}</p>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class="cook-list">
                  <li v-for="(goods,index) in typeGoodsPackage" v-bind:key="index" @click="addOrderList(goods)">
                    <div class="food-img">
                      <img :src="goods.goodsImg" alt="" width="100%">
                    </div>
                    <p class="food-name">{{goods.goodsName}}</p>
                    <p class="food-price">{{goods.price}}</p>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
            </el-col>
          </el-row>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'Pos',
    created() {
      let self = this
      axios.get('https://www.easy-mock.com/mock/5b920cb3f50b092bf9a5a596/pos/goods')
        .then(response => {
          self.typeGoodsStapleFood = response.data.data[0]
          self.typeGoodsCookies = response.data.data[1]
          self.typeGoodsDrinks = response.data.data[2]
          self.typeGoodsPackage = response.data.data[3]
        })
        .catch(error => {
          console.log(error)
        })
    },
    mounted() {
      var orderHeight = document.body.clientHeight
      document.getElementById("order-list").style.height = orderHeight + 'px'
    },
    data () {
      return {
        tableData: [
        ],
        oftenGoods:[
          {
              goodsId:1,
              goodsName:'香辣鸡腿堡',
              price:18
          }, {
              goodsId:2,
              goodsName:'田园鸡腿堡',
              price:15
          }, {
              goodsId:3,
              goodsName:'和风汉堡',
              price:15
          }, {
              goodsId:4,
              goodsName:'快乐全家桶',
              price:80
          }, {
              goodsId:5,
              goodsName:'脆皮炸鸡腿',
              price:10
          }, {
              goodsId:6,
              goodsName:'魔法鸡块',
              price:20
          }, {
              goodsId:7,
              goodsName:'可乐大杯',
              price:10
          }, {
              goodsId:8,
              goodsName:'雪顶咖啡',
              price:18
          }, {
              goodsId:9,
              goodsName:'大块鸡米花',
              price:15
          }, {
              goodsId:20,
              goodsName:'香脆鸡柳',
              price:17
          }
        ],
        typeGoodsStapleFood: [],
        typeGoodsCookies: [],
        typeGoodsDrinks: [],
        typeGoodsPackage: [],
        totalData: [
          {
          "totalPrice": 0
          }
        ],
        // unitPrice: 0
      }
    },
    methods: {
      _totalMoney: function () {
        let self = this
        this.totalData[0].totalPrice = 0;
        this.tableData.forEach(function (elem) {
          // this.totalData[0].totalCount += elem.count
          self.totalData[0].totalPrice += elem.price
        })
      },
      addOrderList: function (goods) {
        this.totalCount = 0
        this.totalMoney = 0
        let existFlag = false
        let self = this
        for(let i=0;i<this.tableData.length;i++) {
          if(this.tableData[i].goodsId === goods.goodsId) {
            existFlag = true
          }
        }
        if(existFlag){
            let arr = this.tableData.filter(function (elem) {
              return (elem.goodsId == goods.goodsId)
            })
            arr[0].count++
            arr[0].price = goods.price * arr[0].count
          }
          else {
            let newGoods = {
              goodsId: goods.goodsId,
              goodsName: goods.goodsName,
              price: goods.price,
              count: 1
            }
            this.tableData.push(newGoods)
          }
          this._totalMoney();
      },
      addFoodCount: function (goods) {
        if (goods.count > 0) {
          goods.price = goods.price + goods.price/goods.count
        }
        else if (goods.count === 0) {
          let arrStapleFood = this.typeGoodsStapleFood.filter(function (elem) {
            return (elem.goodsId === goods.goodsId)
          })
          let arrCookies = this.typeGoodsCookies.filter(function (elem) {
            return (elem.goodsId === goods.goodsId)
          })
          let arrDrinks = this.typeGoodsDrinks.filter(function (elem) {
            return (elem.goodsId === goods.goodsId)
          })
          let arrPackage = this.typeGoodsPackage.filter(function (elem) {
            return (elem.goodsId === goods.goodsId)
          })
          if (arrStapleFood[0]) {
            goods.price = arrStapleFood[0].price
          }
          else if (arrCookies[0]) {
            goods.price = arrCookies[0].price
          }
          else if (arrDrinks[0]) {
            goods.price = arrDrinks[0].price
          }
          else if (arrPackage[0]) {
            goods.price = arrPackage[0].price
          }
          // goods.price = this.unitPrice
        }
        goods.count++
        this._totalMoney();
      },
      minusFoodCount: function (goods) {
        // this.unitPrice = goods.price / goods.count
        if (goods.count < 1) {
          goods = null
        }
        else {
          goods.price = goods.price - goods.price/goods.count
          goods.count--
        }
        this._totalMoney();
      },
      settleAccount: function () {
        alert(this.totalData[0].totalPrice);
      }
    }
  }
</script>

<style lang="scss" scoped>
@import '../style/pos.scss'
</style>
