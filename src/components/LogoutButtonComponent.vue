<template>
<button @click="userLogout" class="logout">Выход</button>
</template>

<script>
export default {
  name: "LogoutButtonComponent",
  emits: [
      'user_logout',
  ],
  methods: {
    userLogout() {
      this.$emit('user_logout');
    }
  },
}
</script>

<style scoped>

</style>