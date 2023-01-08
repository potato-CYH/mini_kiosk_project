<template>
  <div>
    <router-link to="/admin/menus/register">
      <b-button class="order-button w-100" variant="outline-dark">
        메뉴 추가하기
      </b-button>
    </router-link>

    <div @click="moveDetail(menu.id)" v-for="menu in menus" :key="menu.id">
      <div class="menu-container">
        <!-- <img :src="setImage(menu.image_src)" width="200px" alt="메뉴이미지"> -->
        <!-- v-bind를 붙여줘야 vue에서 정의한 data 및 함수 사용 가능-->
        <div
          class="menu-image"
          v-bind:style="`background-image:url(${setImage(menu.image_src)})`"
        ></div>
        <div class="menu-info-wrapper">
          <h2 class="menu-name">{{ menu.name }}</h2>
          <p class="menu-description">{{ menu.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { api } from "../../utils/axios";
export default {
  data() {
    return {
      menus: [],
    };
  },
  async created() {
    this.$store.commit("SET_TITLE", "메뉴 목록");
    const result = await api.menus.findAll();
    console.log(result);
    this.menus = result.data;
  },
  methods: {
    setImage(image_src) {
      let im_src = [...image_src];
      im_src[6] = "/";
      const im = im_src.join("");
      console.log("이미지:" + im);
      
      return `http://13.124.234.249:8081/${im}`;
    },
    moveDetail(id) {
      this.$router.push(`/admin/menus/${id}`);
    },
  },
};
</script>

<style>
.menu-container {
  display: flex;
  align-items: center;
  border-bottom: 3px solid black;
}
.menu-name {
  font-size: 25px;
  font-weight: bold;
}
.menu-description {
  padding-top: 10px;
}
.menu-image {
  background-size: cover;
  background-position: center;
  width: 180px;
  height: 180px;
}
</style>
