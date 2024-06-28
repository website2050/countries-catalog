<template>
  <div class="country-card">
    <img :src="country.flags.png" alt="flag" />
    <h3>{{ country.name.official }}</h3>
    <p>CCA2: {{ country.cca2 }}</p>
    <p>CCA3: {{ country.cca3 }}</p>
    <p>Native Name: {{ nativeName }}</p>
    <p>Alternative Names: {{ altNames }}</p>
    <p>Calling Codes: {{ callingCodes }}</p>
  </div>
</template>

<script>
export default {
  props: {
    country: Object,
  },
  computed: {
    nativeName() {
      const nativeNames = this.country.name.nativeName;
      return nativeNames ? Object.values(nativeNames)[0].common : '';
    },
    altNames() {
      return this.country.altSpellings.join(', ');
    },
    callingCodes() {
      return this.country.idd.root + (this.country.idd.suffixes || []).join(', ');
    },
  },
};
</script>

<style scoped>
.country-card {
  border: 2px solid #181bb5;
  border-radius: 8px;
  padding: 16px;
  margin: 8px;
  background-color: #f0f0f0;
  color: #4a148c;
}
.country-card img {
  max-width: 100px;
  border-radius: 4px;
}
</style>
