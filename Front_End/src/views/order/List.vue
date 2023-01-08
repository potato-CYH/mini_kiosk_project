<template>
  <!--주문 전체 조회-->
  <div class="orderlist">
    <div
      @click="moveDetail(order.id)"
      v-for="order in orderlists"
      :key="order.id"
      class="card bg-light mb-3"
      style="width: 30rem"
    >
      <div class="card-header">주문번호 : {{ order.id }}</div>
      <div class="card-body">
        <h5 class="card-title">주문 메뉴 : {{ order.name }}</h5>
        <p class="card-text">수량 : {{ order.quantity }}</p>
        <p class="card-text">요청 사항 : {{ order.request_detail }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { api } from "../../utils/axios";

export default {
  data() {
    return {
      orderlists: [],
    };
  },
  methods: {
    moveDetail(id) {
      this.$router.push(`/orders/${id}`);
    },
  },
  async created() {
    this.$store.commit("SET_TITLE", "전체 주문 목록");
    const result = await api.orders.findAll();
    console.log(result);
    this.orderlists = result.data;
  },
};
</script>

<style>
.order-container {
  border-bottom: 3px solid black;
}
.orderlist {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
