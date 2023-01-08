<template>
  <!-- 주문 상세 조회-->
  <div class="container" v-if="data.menus_id">
    <div class="card bg-light mb-3" style="width: 30rem">
      <div class="card-header">주문 번호 : {{ this.$route.params.id }}</div>
      <div class="card-body">
        <h5 class="card-title">수량 : {{ data.quantity }}</h5>
        <h5 class="card-title">요청 사항:{{ data.request_detail }}</h5>
      </div>
    </div>
    <div class="btn-group">
      <button
        @click="moveUpdate"
        type="button"
        class="btn btn-primary"
        style="margin: 0 10px 0 10px"
      >
        수정
      </button>
      <button
        @click="deleteOrder"
        type="button"
        class="btn btn-danger"
        style="margin: 0 10px 0 10px"
      >
        삭제
      </button>
      <button
        @click="moveList"
        type="button"
        class="btn btn-info"
        style="margin: 0 10px 0 10px"
      >
        목록
      </button>
    </div>
  </div>
</template>

<script>
import { api } from "@/utils/axios";
export default {
  data() {
    return {
      data: {},
    };
  },
  methods: {
    async deleteOrder() {
      const confirmResult = confirm("삭제하시겠습니까?");
      if (confirmResult) {
        const result = await api.orders.delete(this.$route.params.id);
        alert(result.data.message);
        this.$router.push("/orders"); //이동
      }
    },
    moveUpdate() {
      this.$router.push(`/orders/register/${this.$route.params.id}`);
    },
    moveList() {
      this.$router.push("/orders");
    },
  },
  async created() {
    this.$store.commit("SET_TITLE", "주문 정보");
    console.log(this.$route.params);
    const result = await api.orders.findOne(this.$route.params.id);
    console.log(result);
    this.data = result.data[0];
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.btn-group {
  width: 30rem;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
</style>
