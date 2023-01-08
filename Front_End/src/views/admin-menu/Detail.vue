<template>
  <div>
    <b-card
      v-if="data"
      :title="data.name"
      img-top
      image-alt="디테일"
      tag="article"
      :img-src="setImage()"
      class="mb-2 detail-card"
    >
      <b-card-text>
        {{ data.description }}
      </b-card-text>
      <b-button @click="moveRegister" variant="primary">수정</b-button>
      <b-button @click="deleteMenu" variant="danger">삭제</b-button>
      <b-button @click="moveList" variant="outline-primary">목록</b-button>
    </b-card>
  </div>
</template>

<script>
import { api } from "@/utils/axios";
export default {
  methods: {
    setImage(image_src) {
      return `http://13.124.234.249:8081/${this.data.image_src}`;
    },
    moveRegister() {
      this.$router.push(`/admin/menus/register/${this.$route.params.id}`);
    },
    async deleteMenu() {
      //console.log("삭제 예정")
      const confirmResult = confirm("삭제하시겠습니까?"); // 확인 시 true, 취소 시 false
      if (confirmResult) {
        const result = await api.menus.delete(this.$route.params.id);
        alert(result.data.message);
        this.$router.push("/admin/menus"); //이동
      }
    },
    moveList() {
      this.$router.push("/admin/menus");
    },
  },
  data() {
    return {
      data: {},
    };
  },
  async created() {
    console.log(this.$route.params);
    const result = await api.menus.findOne(this.$route.params.id);
    console.log(result);
    this.data = result.data;
  },
};
</script>

<style>
</style>
