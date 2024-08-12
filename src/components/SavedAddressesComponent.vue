<template>
    <section>Сохранённые адреса</section>
    <div class="address-list" v-for="(savedAddress, id) in savedAddresses">
      <div class="address-item">
        <p>Адрес: {{savedAddress.address}}</p>
      </div>
    </div>
</template>

<script>
export default {
  name: "SavedAddressesComponent",
  props: {
    savedAddresses: {
      type: Array
    }
  }
}
</script>

<style scoped>
 .address-list {
   padding: 10px;
 }
 .address-item {
   border: 2px solid black;
 }
</style>