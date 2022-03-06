<template>
  <div class="template">
    <div class="card-infos">
      <p class="card-author">{{ author }}</p>
      <p class="card-date">{{ date }}</p>
    </div>
    <div class="card-container">
      <div class="card">
        <p class="card-message">
          {{ message }}
        </p>
        <img v-if="image_url" :src="image_url" alt="Une belle image" />
      </div>
      <svg
        @click="clickDelete"
        class="delete"
        style="width: 24px; height: 24px"
        viewBox="0 0 24 24"
      >
        <path
          fill="currentColor"
          d="M19,4H15.5L14.5,3H9.5L8.5,4H5V6H19M6,19A2,2 0 0,0 8,21H16A2,2 0 0,0 18,19V7H6V19Z"
        />
      </svg>
      <svg
        @click="clickModify"
        class="modify"
        style="width: 24px; height: 24px"
        viewBox="0 0 24 24"
      >
        <path
          fill="currentColor"
          d="M20.71,7.04C21.1,6.65 21.1,6 20.71,5.63L18.37,3.29C18,2.9 17.35,2.9 16.96,3.29L15.12,5.12L18.87,8.87M3,17.25V21H6.75L17.81,9.93L14.06,6.18L3,17.25Z"
        />
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",

  props: {
    author: {
      type: String,
    },
    date: {
      type: String,
    },
    message: {
      type: String,
    },
    image_url: {
      type: String,
    },
  },
  created() {},
  methods: {
    clickDelete() {
      this.$emit("clickDelete");
    },
    clickModify() {
      this.$emit("clickModify");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  background: linear-gradient(to right top, #b53dca, #7447f3);
  border-radius: 15px 15px 0px 15px;
  display: flex;
  flex-direction: column;
  width: 15%;
  min-width: 300px;
  padding: 0px 25px;
  color: white;
}
.card-infos {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  width: 100%;
}
.delete,
.modify {
  color: grey;
  cursor: pointer;
}
.card-container {
  display: flex;
  align-items: center;
  margin-bottom: 50px;
  width: 100%;
}
.template {
  max-width: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
img {
  max-width: 300px;
  margin-bottom: 25px;
}
</style>
