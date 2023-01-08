<template>
  <!-- 주문 등록, 수정-->
  <div>
    <div v-if="$route.params.id">
      <div class="card">
        <h5 class="card-header">주문 번호:{{ $route.params.id }}</h5>
        <div class="card-body">
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="inputGroup-sizing-default"
                >수량</span
              >
            </div>
            <input
              type="text"
              class="form-control"
              aria-label="Sizing example input"
              aria-describedby="inputGroup-sizing-default"
              v-model="quantity"
            />
          </div>

          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="inputGroup-sizing-default"
                >요청사항</span
              >
            </div>
            <input
              type="text"
              class="form-control"
              aria-label="Sizing example input"
              aria-describedby="inputGroup-sizing-default"
              v-model="request_detail"
            />
          </div>
          <button
            type="button"
            @click="update(menus_id)"
            class="btn btn-primary"
          >
            주문 수정하기
          </button>
        </div>
      </div>
    </div>
    <div v-for="menu in menus" :key="menu.id">
      <div class="menu-container">
        <!-- <img :src="setImage(menu.image_src)" width="200px" alt="메뉴이미지"> -->
        <!-- v-bind를 붙여줘야 vue에서 정의한 data 및 함수 사용 가능-->
        <div class="menu-wrap">
          <div
            class="menu-image"
            v-bind:style="`background-image:url(${setImage(menu.image_src)})`"
          ></div>
          <div class="menu-info-wrapper">
            <h2 class="menu-name">{{ menu.name }}</h2>
            <p class="menu-description">{{ menu.description }}</p>
          </div>
        </div>
        <b-button variant="primary" v-b-modal="'modal-1' + menu.id"
          >주문하기</b-button
        >

        <b-modal
          :id="'modal-1' + menu.id"
          title="주문하기"
          @ok="create(menu.id)"
        >
          <div class="set1">
            <span class="modal-des">주문 메뉴 :{{ menu.name }}</span>
            수량:<input class="inp1" v-model="quantity" type="text" />
          </div>
          요청사항:<input v-model="request_detail" type="text" />
        </b-modal>
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
      id: "",
      quantity: "",
      request_detail: "",
    };
  },
  async created() {
    if (this.$route.params.id) {
      this.$store.commit("SET_TITLE", "주문 수정하기");
      const result = await api.orders.findOne(this.$route.params.id);
      console.log(result.data[0]);
      this.menus_id = result.data[0].menus_id;
      this.quantity = result.data[0].quantity;
      this.request_detail = result.data[0].request_detail;
    } else {
      this.$store.commit("SET_TITLE", "메뉴 목록");
      const result = await api.menus.findAll();
      console.log(result);
      this.menus = result.data;
    }
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
    async create(id) {
      if (!this.quantity || !this.request_detail) {
        alert("빈 값이 있습니다. 내용르 전부 작성해주세요!");
        return;
      }
      const result = await api.orders.create(
        id,
        this.quantity,
        this.request_detail
      );
      console.log(result);
      //요청 성공
      if (result.data.success) {
        alert(result.data.message);
        this.$router.push("/orders");
      }
      //요청 실패
      if (!result.data.success) {
        alert(result.data.message);
      }
    },
    async update(id) {
      const result = await api.orders.update(
        this.$route.params.id,
        id,
        this.quantity,
        this.request_detail
      );
      console.log(result);
      this.$router.push(`/orders`);
    },
  },
};
</script>

<style>
.menu-container {
  display: flex;
  align-items: center;
  border: 3px solid black;
  margin: 40px;
  justify-content: flex-start;
}
.menu-wrap {
  display: flex;
  align-items: center;
  margin-right: 10%;
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
  margin: 10px;
}
.inp1 {
  width: 10%;
}
.inp2 {
  width: 20%;
}
.set1 {
  display: flex;
  flex-direction: row;
  margin: 10px 0 10px 0;
}
.modal-des {
  margin-right: 10%;
}
.update1 {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
