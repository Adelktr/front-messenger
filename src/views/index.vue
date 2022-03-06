<template>
  <div id="app">
    <div
      class="single-container"
      :key="index"
      v-for="(message, index) in messagesArray"
    >
      <Card
        :author="message.autor"
        :date="message.date"
        :message="message.message"
        :image_url="message.urlImage"
        @clickDelete="deleteMessage(message)"
        @clickModify="modifyMessage(message)"
      />
    </div>
    <form action="POST">
      <label class="formTitle">Add a message</label>
      <div>
        <label>Author<span>*</span></label>
        <input
          :value="messageAuthor != '' ? messageAuthor : ''"
          ref="input1"
          name="author"
          type="text"
          required
        />
      </div>
      <div>
        <label>Message<span>*</span></label>
        <textarea
          :value="messageContent != '' ? messageContent : ''"
          ref="input2"
          name="message"
          type="text"
          required
        />
      </div>
      <div>
        <label>Image url</label>
        <input
          :value="messageImg != '' ? messageImg : ''"
          ref="input3"
          name="urlImage"
          type="text"
        />
      </div>
      <input @click.prevent="getFormValues()" type="submit" value="Send" />
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
      messageAuthor: "",
      messageContent: "",
      messageImg: "",
      messageId: "",
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
      if (this.messageAuthor != "") {
        this.updateMessage();
      } else {
        this.postMessage();
      }
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
        followAllRedirects: true,
      };

      fetch("http://localhost:3000/message", requestOptions)
        .then((response) => response.json())
        .then((data) => {
          this.messagesArray = data;
          location.reload();
        });
    },
    deleteMessage(message) {
      var myHeaders = new Headers();
      myHeaders.append("userid", localStorage.getItem("userId"));
      myHeaders.append(
        "Authorization",
        "Bearer " + localStorage.getItem("token")
      );
      myHeaders.append("Content-Type", "application/json");

      var raw = JSON.stringify({
        msgId: message._id,
      });

      var requestOptions = {
        method: "DELETE",
        headers: myHeaders,
        body: raw,
        redirect: "follow",
      };

      fetch("http://localhost:3000/message/delete", requestOptions)
        .then((response) => response.text())
        .then((result) => console.log(result))
        .then(location.reload())
        .catch((error) => console.log("error", error));
    },
    modifyMessage(message) {
      console.log(message);
      this.messageAuthor = message.autor;
      this.messageContent = message.message;
      this.messageId = message._id;
      if (message.urlImage) {
        this.messageImg = message.urlImage;
      }
    },
    updateMessage() {
      console.log("modification");
      var myHeaders = new Headers();
      myHeaders.append("userid", localStorage.getItem("userId"));
      myHeaders.append(
        "Authorization",
        "Bearer " + localStorage.getItem("token")
      );
      myHeaders.append("Content-Type", "application/json");
      var raw = JSON.stringify({
        msgId: this.messageId,
        message: this.message,
        urlImage: this.messageImg,
      });

      var requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: raw,
        redirect: "follow",
      };
      fetch("http://localhost:3000/message/modify", requestOptions).then(
        location.reload()
      );
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
.single-container {
  max-width: 500px;
  display: flex;
  justify-content: center;
}
</style>
