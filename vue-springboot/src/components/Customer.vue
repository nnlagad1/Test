<template>
  <div v-if="this.customer">
    <h4>Customer</h4>
    <div><label>Name:</label>
	<input type="text" v-model="customer.name" name="name" id="name"/>
	       
    </div>
    <div>
      <label>Age: </label> 
	  <input type="text" v-model="customer.age" name="age" id="age"/>
    </div>
    <div>
      <label>Active: </label> 
	  <input type="text" v-model="customer.active" name="active" id="active"/>
    </div>
  
    <span v-if="this.customer.active"
      v-on:click="updateActive(false,name)"
      class="button is-small btn-primary">Inactive</span>
    <span v-else
      v-on:click="updateActive(true,name)"
      class="button is-small btn-primary">Active</span>
  
    <span class="button is-small btn-danger" v-on:click="deleteCustomer()">Delete</span>
  </div>
  <div v-else>
    <br/>
    <p>Please click on a Customer...</p>
  </div>
</template>

<script>
import http from "../http-common";

export default {
  name: "customer",
  props: ["customer"],
  methods: {
    /* eslint-disable no-console */
    updateActive(status,name) {
      var data = {
        id: this.customer.id,
        name: this.customer.name,
        age: this.customer.age,
        active: status
      };

      http
        .put("/customer/" + this.customer.id, data)
        .then(response => {
          this.customer.active = response.data.active;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },
    deleteCustomer() {
      http
        .delete("/customer/" + this.customer.id)
        .then(response => {
          console.log(response.data);
          this.$emit("refreshData");
          this.$router.push('/');
        })
        .catch(e => {
          console.log(e);
        });
    }
    /* eslint-enable no-console */
  }
};
</script>