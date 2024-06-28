<template>
  <div v-if="isVisible" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content">
      <button class="close-btn" @click="closeModal">X</button>
      <h2>{{ country.name.official }}</h2>
      <img :src="country.flags.png" alt="Flag" />
      <p><strong>2 Character Code:</strong> {{ country.cca2 }}</p>
      <p><strong>3 Character Code:</strong> {{ country.cca3 }}</p>
      <p><strong>Native Name:</strong> {{ getNativeName }}</p>
      <p><strong>Alternative Names:</strong> {{ country.altSpellings.join(', ') }}</p>
      <p><strong>Calling Codes:</strong> {{ getCallingCodes }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    country: Object,
    isVisible: Boolean
  },
  computed: {
    getNativeName() {
      const nativeNames = this.country.name.nativeName;
      return nativeNames ? Object.values(nativeNames).map(name => name.official).join(', ') : 'N/A';
    },
    getCallingCodes() {
      const callingCodes = this.country.idd;
      return callingCodes ? `${callingCodes.root}${callingCodes.suffixes.join(', ')}` : 'N/A';
    }
  },
  methods: {
    closeModal() {
      this.$emit('close');
    }
  }
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  max-width: 500px;
  width: 100%;
  text-align: center;
  position: relative;
}
.close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  background: transparent;
  border: none;
  font-size: 18px;
  cursor: pointer;
}
</style>
