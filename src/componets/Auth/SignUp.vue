<template>
  <div class="wrapper">
    <div class="sign-up-form">
      <span class="title">Sign Up</span>
      <form @submit.prevent="registerUser">
        <div>
          <input v-model="user.email" required placeholder="Enter mail" type="email" />
        </div>
        <div>
          <input v-model="user.password" required placeholder="Create a password" type="password" />
        </div>
        <div>
          <input
            v-model="user.confirmPassword"
            required
            placeholder="Confirm password"
            type="password"
          />
        </div>

        <button class="submit-btn" type="submit">SIGN UP</button>
      </form>
    </div>
    <div class="error" role="alert" v-if="error">
      <strong>Oops! Passwords do not match or you forgot to enter them</strong>
    </div>
  </div>
</template>

<script>
export default {
  name: "SignUp",
  data() {
    return {
      user: {
        email: "",
        password: "",
        confirmPassword: ""
      },
      error: false
    };
  },

  methods: {
    registerUser() {
      if (
        this.user.password !== this.user.confirmPassword ||
        this.user.password.length < 6
      ) {
        this.error = true;
      } else {
        firebase
          .auth()
          .createUserWithEmailAndPassword(this.user.email, this.user.password)
          .then(() => {
            this.$emit("regSuccess", "sign-in");
          })
          .catch(error => console.log("Возникла ошибка: " + error));
      }
    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.sign-up-form {
  background: #1f2027;
  width: 380px;
  height: 350px;
  display: flex;
  flex-direction: column;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.35);
}

.title {
  font-size: 24px;
  text-transform: uppercase;
  font-weight: 500;
  color: #ffffff;
  margin-top: 40px;
  margin-left: 30px;
  margin-bottom: 20px;
}

input {
  padding-left: 10px;
  color: #ffffff;
  font-size: 16px;
  outline: none;
  width: 320px;
  height: 44px;
  margin: 0px 29px;
  margin-bottom: 10px;
  background: #505050;
  border: 1px solid rgba(31, 32, 65, 0.25);
  box-sizing: border-box;
  border-radius: 4px;
}

.submit-btn {
  cursor: pointer;
  color: #ffffff;
  font-weight: bold;
  margin: 10px 20px 29px 29px;
  width: 320px;
  height: 44px;
  background: #4ba467;
  border: 1px solid rgba(31, 32, 65, 0.25);
  box-sizing: border-box;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

.error {
  padding-top: 20px;
  max-width: 310px;
  color: #fe6b5b;
  margin: 0 auto;
  text-align: center;
  font-size: 16px;
}

.submit-btn:hover {
  background: #3bca68;
  transition: all 0.3s ease-in-out;
}
</style>
