<template>
<div class="form">
  <section>Вход</section>
  <div>
    <input placeholder="Email" type="email"  v-model="loginUser.email">
  </div>
  <div>
    <input placeholder="Пароль" type="password" v-model="loginUser.password">
  </div>
  <div>
    <button type="submit" @click="userLogin">Войти</button>
  </div>
</div>
</template>

<script>
export default {
  name: "LoginFormComponent",
  emits: [
    'user_login',
  ],
  data() {
    return {
      loginUser: {
        email: '',
        password: '',
      }
    }
  },
  methods: {
    userLogin() {
      if (this.loginUser.email && this.loginUser.password) {
        this.$emit('user_login', this.loginUser);
        this.loginUser = {
          email: '',
          password: '',
        }
      }
    },
  },
}
</script>

<style scoped>

</style>