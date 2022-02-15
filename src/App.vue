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
        <customer-table :customers='customers' @edit-customer="editCustomer" @delete-customer="deleteCustomer" />
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
      ]
    }
  },
  methods: {
    addCustomer(customer) {
      this.customers.push(customer);
    },
    editCustomer(id, data) {
      this.customers = this.customers.map(function(customer) { 
        return customer.id === id ? data : customer;
      });
    },
    deleteCustomer(id) {
      this.customers = this.customers.filter(function(customer) { 
        return customer.id !== id;
      });
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