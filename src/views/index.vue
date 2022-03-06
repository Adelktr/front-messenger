<template>
  <div id="app">
    <div :key="index" v-for="(message, index) in messagesArray">
      <Card
        :author="message.autor"
        :date="message.date"
        :message="message.message"
        :image_url="message.urlImage"
      />
    </div>
    <form action="POST">
      <label class="formTitle">Add a message</label>
      <div>
        <label>Author<span>*</span></label>
        <input ref="input1" name="author" type="text" required />
      </div>
      <div>
        <label>Message<span>*</span></label>
        <textarea ref="input2" name="message" type="text" required />
      </div>
      <div>
        <label>Image url</label>
        <input ref="input3" name="urlImage" type="text" />
      </div>
      <input @click="getFormValues()" type="submit" value="Send" />
    </form>
  </div>
</template>

<script>
import Card from "../components/Card.vue";

export default {
  name: "Index",
  components: {
    Card,
  },
  data() {
    return {
      messagesArray: [],
      author: String,
      message: String,
      urlImage: String,
    };
  },
  methods: {
    fetchUrl() {
      var myHeaders = new Headers();
      myHeaders.append("userid", localStorage.getItem("userId"));
      myHeaders.append(
        "Authorization",
        "Bearer " + localStorage.getItem("token")
      );

      var requestOptions = {
        method: "GET",
        headers: myHeaders,
        redirect: "follow",
      };

      fetch("http://localhost:3000/message", requestOptions)
        .then((response) => response.json())
        .then((data) => {
          this.messagesArray = data;
        });
    },
    getFormValues() {
      this.author = this.$refs.input1.value;
      this.message = this.$refs.input2.value;
      this.urlImage = this.$refs.input3.value;
      this.postMessage();
    },
    postMessage() {
      var myHeaders = new Headers();
      myHeaders.append("userid", localStorage.getItem("userId"));
      myHeaders.append(
        "Authorization",
        "Bearer " + localStorage.getItem("token")
      );
      myHeaders.append("Content-Type", "application/json");

      var raw = JSON.stringify({
        autor: this.author,
        message: this.message,
        date: new Date(),
        urlImage: this.urlImage,
      });

      var requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: raw,
        redirect: "follow",
      };

      fetch("http://localhost:3000/message", requestOptions)
        .then((response) => response.json())
        .then((data) => {
          this.messagesArray = data;
        });
    },
  },
  created() {
    this.fetchUrl();
  },
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
form {
  position: fixed;
  left: 50px;
  display: flex;
  flex-direction: column;
  margin-top: 50px;
  gap: 25px;
  width: 25%;
}
form > div {
  display: flex;
  flex-direction: column;
}
textarea {
  resize: none;
  height: 100px;
}
span {
  color: red;
}
.formTitle {
  font-size: 28px;
}
</style>
