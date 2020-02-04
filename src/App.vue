<template>
  <div id="app">
    <!-- <SignUp></SignUp> -->
    <div class="btn-container" v-if="!isMainPage">
      <div v-if="!signComplete">
        <button class="sign-up" @click="switchSign('sign-up')">Sign Up</button>
        <button class="sign-in" @click="switchSign('sign-in')">Sign In</button>
      </div>
    </div>
    <span v-else>{{ email }}</span>
    <div v-if="!isMainPage">
      <SignIn
        v-if="sign === 'sign-in'"
        @addUser="isMainPage = $event.mainPage, signComplete = $event.complete, email = $event.email, uid = $event.uid"
      ></SignIn>
      <SignUp v-else @regSuccess="sign = $event"></SignUp>
    </div>
    <div v-else>
      <div>
        <TodoList :uid="uid"></TodoList>
      </div>
    </div>
  </div>
</template>

<script>
import TodoList from "./componets/TodoList";
import SignUp from "./componets/Auth/SignUp";
import SignIn from "./componets/Auth/SignIn";

export default {
  name: "app",
  components: {
    TodoList: TodoList,
    SignUp: SignUp,
    SignIn: SignIn
  },

  data() {
    return {
      sign: "sign-in",
      isMainPage: false,
      signComplete: false,
      email: "",
      uid: ""
    };
  },

  methods: {
    switchSign(currentSign) {
      this.sign = currentSign;
    }
  }
};
</script>

<style>
button {
  cursor: pointer;
}
body {
  font-family: "Roboto", sans-serif;
  font-size: 16px;
  background-color: #35383f;
}

.btn-container {
  display: flex;
  justify-content: center;
}

.sign-up,
.sign-in {
  cursor: pointer;
  color: #ffffff;
  font-weight: bold;
  margin: 10px 20px 29px 29px;
  width: 320px;
  height: 44px;
  border: 1px solid rgba(31, 32, 65, 0.25);
  box-sizing: border-box;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
  text-transform: uppercase;
}

.sign-in {
  background: #4ba467;
}

.sign-in:focus {
  border: 1px solid #40835d;
  background: #3bca68;
  transition: all 0.3s ease-in-out;
}

.sign-up {
  background: #f4c951;
}

.sign-up:focus {
  border: 1px solid #a6a836;
  background: #fac22a;
  transition: all 0.3s ease-in-out;
}
</style>
