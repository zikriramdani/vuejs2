<template>
  <div class="customer-form">
    <form @submit.prevent="submit">
      <div class="form-group mb-3">
        <label for="name">Nama</label>
        <input type="text" v-model="customer.name"
        id="name" name="name" class="form-control"
        :class="{ 'is-invalid': submitted && $v.customer.name.$error }" placeholder="Masukan name">
        <div v-if="submitted && !$v.customer.name.required" class="invalid-feedback">Name is required</div>
      </div>
      <div class="form-group mb-3">
        <label for="email">Email</label>
        <input type="email" v-model="customer.email"
        id="email" name="email" class="form-control"
        :class="{ 'is-invalid': submitted && $v.customer.email.$error }" placeholder="Masukan email">
        <div v-if="submitted && $v.customer.email.$error" class="invalid-feedback">
          <span v-if="!$v.customer.email.required">Email is required</span>
          <span v-if="!$v.customer.email.email">Email is invalid</span>
        </div>
      </div>
      <div class="form-group mb-3">
        <label for="address">Alamat</label>
        <textarea type="text" v-model="customer.address"
        id="address" name="address" class="form-control"
        :class="{ 'is-invalid': submitted && $v.customer.address.$error }" placeholder="Masukan address"></textarea>
        <div v-if="submitted && !$v.customer.address.required" class="invalid-feedback">Address is required</div>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
import { required, email, minLength } from "vuelidate/lib/validators";

export default {
  name: 'customer-form',
  data () {
    return {
      customer: {
        name: "",
        email: "",
        address: ""
      },
      submitted: false
    }
  },
  validations: {
    customer: {
      name: { required },
      email: { required, email },
      address: { required, minLength: minLength(6) }
    }
  },
  methods: {
    submit() {
      this.submitted = true;
      // stop here if form is invalid
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }

      // alert("SUCCESS!! :-)\n\n" + JSON.stringify(this.customer));
      this.$emit('add-customer', this.customer)
      console.log('add', this.customer)
      this.customer = {
        name: "",
        email: "",
        address: ""
      }
    }
  }
}
</script>
