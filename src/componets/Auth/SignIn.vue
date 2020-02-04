<template>
  <div class="wrapper">
    <div class="sign-up-form">
      <span class="title">Sign In</span>
      <form @submit.prevent="enterUser">
        <div>
          <input v-model="user.email" required placeholder="Enter mail" type="email" />
        </div>
        <div>
          <input v-model="user.password" required placeholder="Enter password" type="password" />
        </div>
        <button class="submit-btn" type="submit">SIGN IN</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {
        email: "",
        password: ""
      }
    };
  },

  methods: {
    enterUser() {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.user.email, this.user.password)
        .then(response => {
          const sett = {
            email: response.email,
            uid: response.user.uid,
            mainPage: true,
            complete: true
          };
          this.$emit("addUser", sett);
        });
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

.sign-up-form:hover {
  background: #17171b;
  transition: all 0.3s ease-in;
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

.question {
  margin-top: 16px;
  color: #76777e;
  margin-left: 29px;
}

.link-to-auth {
  width: 350px;
  display: flex;
  justify-content: space-between;
}

.sign-up {
  cursor: pointer;
  color: #ffffff;
  font-size: 14px;
  font-weight: bold;
  text-transform: uppercase;
  background: none;
  width: 100px;
  height: 44px;
  border: 1px solid #f4c951;
  border-radius: 4px;
}
.sign-up:hover {
  border: none;
  background: #fac22a;
  transition: all 0.3s ease-in-out;
}

.submit-btn:hover {
  background: #3bca68;
  transition: all 0.3s ease-in-out;
}
</style>
