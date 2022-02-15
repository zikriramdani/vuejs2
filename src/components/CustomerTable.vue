<template>
  <div class="table-responsive">
    <table class="table table-striped">
      <thead class="thead-light">
        <tr>
          <th scope="col">No</th>
          <th scope="col">Nama</th>
          <th scope="col">Email</th>
          <th scope="col">Alamat</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(customer, index) in customers" :key="customer.id">
          <template v-if="customerId === customer.id">
            <td>
              <label for="name" class="font-weight-bold">No:</label>
              <div> 
                <input type="text" :value="index+1" class="form-control" disabled />
              </div>
            </td>
            <td>
              <label for="name" class="font-weight-bold">Nama:</label>
              <div class="form-group"> 
                <input type="text" v-model="customer.name"
                id="name" name="name" class="form-control" placeholder="Masukan name" />
              </div>
            </td>
            <td>
              <label for="email" class="font-weight-bold">Email:</label>
              <div class="form-group"> 
                <input type="email" v-model="customer.email"
                id="email" name="email" class="form-control" placeholder="Masukan email" />
              </div>
            </td>
            <td>
              <label for="address" class="font-weight-bold">Address:</label>
              <div class="form-group"> 
                <textarea type="text" v-model="customer.address"
                id="address" name="address" class="form-control" placeholder="Masukan address"></textarea>
              </div>
            </td>
            <td class="align-bottom">
              <div class="d-flex justify-content-end">
                <button type="submit" class="btn btn-success me-3" @click="saveEdit(customer)">Save</button>
                <button class="btn btn-danger" @click="cancelEdit(customer)" >Cancel</button>
              </div>
            </td>
          </template>
          <template v-else>
            <td class="align-middle">{{index+1}}</td>
            <td class="align-middle">{{customer.name}}</td>
            <td class="align-middle">{{customer.email}}</td>
            <td class="align-middle">{{customer.address}}</td>
            <td>
              <div class="d-flex justify-content-end">
                <button class="btn btn-primary me-3" @click="editCustomer(customer)">Edit</button> 
                <button class="btn btn-danger" @click="deleteCustomer(customer.id)">Hapus</button> 
              </div>
            </td>
          </template>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>

export default {
  name: 'customer-table',
  props: {
    customers: []
  },
  data () {
    return {
      customerId: null,
      customer: {
        name: "",
        email: "",
        address: ""
      },
    }
  },
  methods: {
    editCustomer(customer) {
      this.data = Object.assign({}, customer);
      this.customerId = customer.id;
      console.log('editCustomer',this.data);
    },
    saveEdit(customer) {
      // alert("SUCCESS!! :-)\n\n" + JSON.stringify(customer));
      const id = this.data.id;
      this.$emit('edit-customer', id, customer);
      this.customerId = null;
    },
    cancelEdit(customer) {
      Object.assign(customer, this.data);
      this.customerId = null;
      console.log('cancelEdit',this.customerId);
    },
    deleteCustomer(id){
      this.$emit('delete-customer', id);
      console.log('delete', id)
    }
  }
}
</script>
