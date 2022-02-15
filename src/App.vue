<template>
  <div id="app">
    <div class="container">
      <div class="col-12">
        <h1 class="mb-4">Data</h1> 
        <hr>
        <p> 
          <button class="btn btn-primary" type="button" 
          data-bs-target="#collapseExample" 
          data-bs-toggle="collapse">
            Tambah Data
          </button>
        </p>
      </div>
      <div class="col-12">
        <div>
          <input type="text" class="form-control" v-model="search" placeholder="Search name.."/>
        </div>
        <customer-table 
          :customers="filteredCustomers"
          @edit-customer="editCustomer" 
          @delete-customer="deleteCustomer" 
          @sort-no="sort('id')"
          @sort-name="sort('name')"
          @sort-email="sort('email')"
          @sort-address="sort('address')"
        />
        debug: sort={{currentSort}}, dir={{currentSortDir}}, page={{currentPage}}
        <div class="d-flex justify-content-center mb-3">
          <button @click="prevPage()">Previous</button> 
          <button @click="nextPage()">Next</button>
        </div>
      </div>

      <div class="collapse" id="collapseExample">
        <div class="card card-body">
          <h2>Input Data</h2>
          <hr>
          <customer-form @add-customer="addCustomer" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CustomerTable from '@/components/CustomerTable.vue'
import CustomerForm from '@/components/CustomerForm.vue'

export default {
  name: 'App',
  components: {
    CustomerTable,
    CustomerForm,
  },
  data() {
    return {
      customers: [
        {
          id: 1,
          name: 'Zikri',
          email: 'zikri@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 2,
          name: 'Ramdani',
          email: 'ramdani@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 3,
          name: 'Agus',
          email: 'agus@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 4,
          name: 'Zikri',
          email: 'zikri@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 5,
          name: 'Ramdani',
          email: 'ramdani@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 6,
          name: 'Agus',
          email: 'agus@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 7,
          name: 'Zikri',
          email: 'zikri@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 8,
          name: 'Ramdani',
          email: 'ramdani@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 9,
          name: 'Agus',
          email: 'agus@gmail.com',
          address: 'Jl H Mair',
        },
        {
          id: 10,
          name: 'Agus',
          email: 'agus@gmail.com',
          address: 'Jl H Mair',
        },
      ],
      currentSort: 'name',
      currentSortDir: 'asc',
      pageSize: 3,
      currentPage: 1,
      search: '',
    }
  },
  computed:{
    filteredCustomers() {
      let tempCustomers = this.customers;

      if (this.search != '' && this.search) {
        tempCustomers = tempCustomers.filter(customer => {
          return customer.name.toLowerCase().includes(this.search.toLowerCase())
        })
      }

      tempCustomers = tempCustomers.sort((a,b) => {
        let modifier = 1;
        if(this.currentSortDir === 'desc') modifier = -1;
        if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;
      }).filter((row, index) => {
        const start = (this.currentPage-1) * this.pageSize;
        const end = this.currentPage * this.pageSize;
        if(index >= start && index < end) return true;
      });

      return tempCustomers;
    },
  },
  methods: {
    addCustomer(customer) {
      this.customers.push(customer);
    },
    editCustomer(id, data) {
      this.customers = this.customers.map((customer) => { 
        return customer.id === id ? data : customer;
      });
    },
    deleteCustomer(id) {
      this.customers = this.customers.filter((customer) => { 
        return customer.id !== id;
      });
    },

		sort(s) {
      console.log('a', s)
      if(s === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
      }
      this.currentSort = s;
    },
    nextPage() {
      if((this.currentPage*this.pageSize) < this.customers.length) this.currentPage++;
    },
    prevPage() {
      if(this.currentPage > 1) this.currentPage--;
    }
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>