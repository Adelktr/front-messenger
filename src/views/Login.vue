<template>
  <div class="login">
    LOGIN
    <form action="POST">
      <div>
        <label>Mail</label>
        <input ref="emailInput" type="text" />
      </div>
      <div>
        <label>Password</label>
        <input ref="passwordInput" type="password" />
      </div>
      <input @click.prevent="getFormValues()" type="submit" />
    </form>
    <p v-if="displayError">User or password doesn't match!</p>
  </div>
</template>
<script>
import router from "../router/index";
export default {
  name: "Login",
  components: {},
  data() {
    return {
      email: String,
      password: String,
      displayError: false,
    };
  },
  methods: {
    getFormValues() {
      this.email = this.$refs.emailInput.value;
      this.password = this.$refs.passwordInput.value;
      this.verifyUser();
    },
    verifyUser() {
      // let headers = new Headers();
      // headers.append("Content-Type", "application/json");
      // let raw = JSON.stringify({
      //   email: this.email,
      //   password: this.password,
      // });
      // let requestOptions = {
      //   method: "POST",
      //   headers: headers,
      //   body: raw,
      //   redirect: "follow",
      // };
      // fetch("localhost:3000/user/login", requestOptions)
      //   .then((response) => response.json())
      //   .then((data) => {
      //     console.log(data);
      //   })
      //   .catch((error) => console.log("error", error));

      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/json");

      var raw = JSON.stringify({
        email: this.email,
        password: this.password,
      });

      var requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: raw,
        redirect: "follow",
      };

      fetch("http://localhost:3000/user/login", requestOptions)
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          if (data.userId && data.token) {
            let ls = localStorage;
            ls.setItem("userId", data.userId);
            ls.setItem("token", data.token);
            router.push("/index");
          } else {
            this.displayError = true;
          }
        });
    },
  },
};
</script>
<style scoped>
.login {
  display: flex;
  flex-direction: column;
  align-items: center;
}
form {
  display: flex;
  flex-direction: column;
  margin-top: 50px;
  gap: 25px;
}
form > div {
  display: flex;
  flex-direction: column;
}
</style>
