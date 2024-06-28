<template>
  <div class="pagination">
    <button :disabled="currentPage === 1" @click="changePage(currentPage - 1)">Previous</button>
    <span>Page {{ currentPage }}</span>
    <button :disabled="currentPage === totalPages" @click="changePage(currentPage + 1)">Next</button>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: Number,
    totalItems: Number,
    itemsPerPage: Number,
  },
  computed: {
    totalPages() {
      return Math.ceil(this.totalItems / this.itemsPerPage);
    },
  },
  methods: {
    changePage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.$emit('page-changed', page);
      }
    },
  },
};
</script>

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}
button {
  padding: 8px 16px;
  background-color: #4a148c;
  color: white;
  border: none;
  cursor: pointer;
  margin: 0 10px;
}
button:disabled {
  background-color: #e0e0e0;
  cursor: not-allowed;
}
span {
  align-self: center;
}
</style>
