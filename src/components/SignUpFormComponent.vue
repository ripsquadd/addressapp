<template>
  <div class="form">
    <section>Регистрация</section>
    <div>
      <input placeholder="Имя пользователя" type="text" v-model="newUser.name">
    </div>
    <div>
      <input placeholder="Email" type="email" v-model="newUser.email">
    </div>
    <div>
      <input placeholder="Пароль" type="password" v-model="newUser.password">
    </div>
    <div>
      <button type="submit" @click="userSignUp">Регистрация</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "SignUpFormComponent",
  emits: [
    'user_signup',
  ],
  data() {
    return {
      newUser: {
        name: '',
        email: '',
        password: '',
      }
    }
  },
  methods: {
    userSignUp() {
      if (this.newUser.name && this.newUser.email && this.newUser.password) {
        this.$emit('user_signup', this.newUser);
        this.newUser = {
          name: '',
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