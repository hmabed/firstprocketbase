<script setup>
import PocketBase from 'pocketbase'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div>
      <label width="50px">Login    : </label><input type="email" id="login">
      <p></p>
      <label width="50px">Password : </label><input type="password" id="passwd">
      <p></p>
      <hr>
      <button v-on:click="register()">Register</button>
      <button v-on:click="login()">Login</button><p></p>
      <button v-on:click="forget()">Forget password</button>
    </div>
  </header>

  <main>
    <!-- <TheWelcome /> -->
  </main>
</template>

<script>
const pb = new PocketBase('http://env-6409486.sh1.hidora.net:80')
export default {
  methods: {
    //this method allows a new user to sign up the system. Once done, the user receives an email
    //asking for account validation. Once the validation made the user is added to the system
    async login() {
        await pb.collection('users').authWithPassword(document.getElementById("login").value,
        document.getElementById("passwd").value);
    },
    //this method allows the already registred user to log in the system.
    async register() {
      await pb.collection('users').create({
        email: document.getElementById("login").value,
        password: document.getElementById("passwd").value,
        passwordConfirm: document.getElementById("passwd").value,
        name: 'John Di',
      });
    },
    async forget() {
      await pb.collection('users').requestPasswordReset(document.getElementById("login").value);
    }
  }
}
</script>
<style scoped>

label {
  width:100px;
  display: inline-block;
}
button {
  width:150px;
}
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
