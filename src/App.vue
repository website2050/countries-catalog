<template>
  <div id="app">
    <header class="header">
      <h1>Countries Catalog</h1>
      <input v-model="searchQuery" placeholder="Search by Country Name" class="search-input" />
      <button @click="sortCountries('asc')" class="sort-button">Sort Ascending</button>
      <button @click="sortCountries('desc')" class="sort-button">Sort Descending</button>
    </header>
    <div class="countries-list">
      <CountryCard
        v-for="country in paginatedCountries"
        :key="country.cca3"
        :country="country"
        @click="openModal(country)"
        class="country-card"
      />
    </div>
    <PaginationComponent
      :current-page="currentPage"
      :total-items="filteredCountries.length"
      :items-per-page="itemsPerPage"
      @page-changed="changePage"
      class="pagination"
    />
    <CountryModal
      v-if="selectedCountry"
      :country="selectedCountry"
      :is-visible="isModalVisible"
      @close="isModalVisible = false"
    />
  </div>
</template>

<script>
import CountryCard from './components/CountryCard.vue';
import CountryModal from './components/CountryModal.vue';
import PaginationComponent from './components/PaginationComponent.vue';
import Api from './services/api';

export default {
  name: 'App',
  components: {
    CountryCard,
    CountryModal,
    PaginationComponent,
  },
  data() {
    return {
      countries: [],
      searchQuery: '',
      currentPage: 1,
      itemsPerPage: 25,
      sortDirection: 'asc',
      selectedCountry: null,
      isModalVisible: false,
    };
  },
  computed: {
    filteredCountries() {
      const query = this.searchQuery.toLowerCase();
      return this.countries.filter(country =>
        country.name.official.toLowerCase().includes(query)
      );
    },
    sortedCountries() {
      return [...this.filteredCountries].sort((a, b) => {
        const nameA = a.name.official.toLowerCase();
        const nameB = b.name.official.toLowerCase();
        if (this.sortDirection === 'asc') {
          return nameA < nameB ? -1 : nameA > nameB ? 1 : 0;
        }
        return nameA > nameB ? -1 : nameA < nameB ? 1 : 0;
      });
    },
    paginatedCountries() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.sortedCountries.slice(start, end);
    },
  },
  methods: {
    fetchCountries() {
      Api.getAllCountries()
        .then(response => {
          this.countries = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
    sortCountries(direction) {
      this.sortDirection = direction;
    },
    changePage(page) {
      this.currentPage = page;
    },
    openModal(country) {
      this.selectedCountry = country;
      this.isModalVisible = true;
    },
  },
  created() {
    this.fetchCountries();
  },
};
</script>

<style scoped>
#app {
  
  font-family: 'Arial', sans-serif;
  background-color: #f0f0f0;
  color: #4a148c; 
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header {
  width: 100%;
  background-color: #7b1fa2;
  color: white;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.search-input {
  padding: 8px;
  margin-right: 16px;
  border: 1px solid #ba68c8; 
  border-radius: 4px;
  background-color: #ffffff; 
  color: #4a148c; 
  font-size: 14px;
}

.sort-button {
  padding: 8px 16px;
  background-color: #ba68c8; 
  color: #ffffff; 
  border: none;
  cursor: pointer;
  border-radius: 4px;
  font-size: 14px;
}

.sort-button:hover {
  background-color: #9c27b0; 
}

.countries-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 20px;
}

.country-card {
  margin: 10px;
}

.pagination {
  margin-top: 20px;
}

.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
  z-index: 1000;
  max-width: 80%;
  max-height: 80%;
  overflow-y: auto;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #ddd;
  padding-bottom: 10px;
  margin-bottom: 10px;
}

.modal-title {
  font-size: 18px;
  font-weight: bold;
  color: #4a148c;
}

.modal-close {
  cursor: pointer;
  font-size: 18px;
  color: #757575; 
}

.modal-close:hover {
  color: #4a148c; 
}


.modal-content p {
  margin-bottom: 10px;
  line-height: 1.5;
}

.modal-content img {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
  margin-bottom: 10px;
}

@media (max-width: 600px) {
  .modal {
    max-width: 95%;
  }
}

</style>
