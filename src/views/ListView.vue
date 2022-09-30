<template>
  <el-container>
    <el-main>
      <el-button type="primary" @click="dialogVisible = true">我的购物车</el-button>
      <el-table :data="productdata" style="width: 100%">
        <el-table-column prop="id" label="编号" width="180"></el-table-column>
        <el-table-column prop="price" label="单价" width="180"></el-table-column>
        <el-table-column prop="stock" label="库存" width="180"></el-table-column>
        <el-table-column prop="lastUpdateTime" label="最后购买时间" width="180"></el-table-column>
        <el-table-column label="购买数量" width="400">
          <template slot-scope="scope">
            <el-button type="primary" @click="toinsert(scope.row)">加入购物车</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-dialog title="我的购物车" :visible.sync="dialogVisible" width="100%">
        <el-table :data="orderdata" style="width: 100%">
          <el-table-column prop="orderId" label="订单编号" width="180"></el-table-column>
          <el-table-column prop="userId" label="用户id" width="180"></el-table-column>
          <el-table-column prop="productId" label="商品id" width="180"></el-table-column>
          <el-table-column prop="count" label="数量" width="180"></el-table-column>
          <el-table-column prop="totalPrice" label="总价" width="180"></el-table-column>
          <el-table-column label="操作" width="400">
            <template slot-scope="scope">
              <el-button type="primary" @click="tosave(scope.row)">支付</el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-dialog>
    </el-main>
  </el-container>
</template>

<script>
export default {
  name: 'ListView',
  data () {
    return {
      dialogVisible: false,
      ordertoltal:0,
      orderdata: [],
      productdata: [],
      form: {
        userId: '',
        productId: '',
        count: 1
      },
      shoping: {
        userId: 0,
        productId: 0,
        count: 0,
        totalPrice: 0
      }
    }
  },
  created () {
    this.query()
  },
  methods: {
    query () {
      this.axios.post('http://localhost:9001/product/product/list').then(res => {
        this.productdata = res.data
      })
    },
    toinsert (scope) {
      this.shoping.count = 1
      this.shoping.userId = this.$store.state.userId
      this.shoping.productId = scope.id
      this.shoping.totalPrice = scope.price
      this.axios.post('http://localhost:9001/pay/shoping/add',this.shoping).then(res => {
        this.orderdata = res.data
      })
    },
    tosave (scope) {
      this.axios.post('http://localhost:9001/pay/order/insert', scope).then(res => {
        if (res.data != null && res.data != '') {
          window.open('http://localhost:9001/pay/order/pay?orderid=' + res.data.id
                  + '&ordertotal=' + res.data.totalPrice)
        } else {
          this.$message.error('生成订单失败')
        }
      })
    }
  }
}
</script>

<style scoped>

</style>
