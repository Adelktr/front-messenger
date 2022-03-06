<template>
  <div>
    <div class="card-infos">
      <p class="card-author">{{ author }}</p>
      <p class="card-date">{{ date }}</p>
      <v-icon>mdi-delete</v-icon>
    </div>
    <div class="card">
      <p class="card-message">
        {{ message }}
      </p>
      <img
        v-if="image_url"
        :src="image_url"
        alt="Pas d'alt, fallait pas être aveugle"
      />
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
  margin-bottom: 50px;
}
.card-infos {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
}
</style>
