<template>
  <el-container>
    <el-main>
      <el-table :data="orderdata" style="width: 100%">
        <el-table-column prop="orderId" label="订单编号" width="180"></el-table-column>
        <el-table-column prop="userId" label="用户id" width="180"></el-table-column>
        <el-table-column prop="productId" label="商品id" width="180"></el-table-column>
        <el-table-column prop="count" label="数量" width="180"></el-table-column>
        <el-table-column prop="totalPrice" label="总价" width="180"></el-table-column>
        <el-table-column prop="status" label="状态" width="180"></el-table-column>
        <el-table-column prop="addTime" label="订单生成时间" width="180"></el-table-column>
      </el-table>
    </el-main>
  </el-container>
</template>

<script>
export default {
  name: 'OrderView',
  data () {
    return {
      orderdata: []
    }
  },
  created () {
    this.query()
  },
  methods: {
    query () {
      this.axios.post('http://localhost:9001/pay/order/list').then(res => {
        this.orderdata = res.data
      })
    }
  }
}
</script>

<style scoped>

</style>
