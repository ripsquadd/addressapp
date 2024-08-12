<template>
  <section>Найденные адреса</section>
  <div class="address-list" v-for="(outerAddresses, id) in outerAddresses">
    <div class="address-item">
      <p>Идентификатор: {{id}}</p>
      <p>Адрес: {{outerAddresses.unrestricted_value}}</p>
      <button class="address-save-btn" @click="addressSave(id)">Сохранить Адрес</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "OuterAddressesListComponent",
  emits: [
    'address_save'
  ],
  props: {
    outerAddresses: {
      type: Array
    },
    currentUserId: {
      type: String
    }
  },
  data() {
    return {
      newAddress: {
        address: '',
        user_id: null,
      }
    }
  },
  methods: {
    addressSave(id) {
    this.newAddress.address = this.outerAddresses[id].unrestricted_value;
    this.newAddress.user_id = Number(this.currentUserId);
      this.$emit('address_save', this.newAddress);
      this.newAddress = {
        address: '',
        user_id: null,
      }
    }
  },
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