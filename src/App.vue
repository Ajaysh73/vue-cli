<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Http Resource Demonstration</h1>
        <div class="form-group">
          <label for>Customer Name:</label>
          <input type="text" name id class="form-control" v-model="customer.customerName">
        </div>
        <div class="form-group">
          <label for>Phone:</label>
          <input type="text" name id class="form-control" v-model="customer.customerPhone">
        </div>
        <div class="form-group">
          <label for>Data Node:</label>
          <input type="text" name id class="form-control" v-model="node">
        </div>
        <button class="btn btn-primary" @click="submit">Post Data</button>
      </div>
    </div>
    <br>
    <br>
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <button class="btn btn-primary" @click="getData">Get Data</button>
        <br>
        <br>
        <ul class="list-group">
          <li
            class="list-item"
            v-for="customer in customers"
            :key="customer._id"
          >{{customer.customerName}} - {{customer.customerPhone}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      customer: {
        customerName: "",
        customerPhone: ""
      },
      customers: [],
      node: "customerData",
      resource: {}
    };
  },
  methods: {
    submit() {
      //   this.$http.post("customerData.json", this.customer).then(
      //     response => {
      //       console.log(response);
      //     },
      //     error => {
      //       console.log(error);
      //     }
      //   );
      //   this.resource.save({}, this.customer);
      this.resource.saveAlt(this.customer);
      //   https://vueresourcedemo.firebaseio.com/
    },
    getData() {
      //   this.$http
      //     .get("customerData.json")
      //     .then(response => {
      //       return response.json();
      //     })
      //     .then(data => {
      //       const custArray = [];
      //       for (let key in data) {
      //         custArray.push(data[key]);
      //       }
      //       this.customers = custArray;
      //     });
      // }
      this.resource
        .getData({ node: this.node })
        .then(response => {
          return response.json();
        })
        .then(data => {
          const custArray = [];
          for (let key in data) {
            custArray.push(data[key]);
          }
          this.customers = custArray;
        });
    }
  },
  created() {
    const customActions = {
      saveAlt: { method: "POST", url: "alternative.json" },
      getData: { method: "GET" }
    };
    this.resource = this.$resource("{node}.json", {}, customActions);
  }
};
</script>

<style>
</style>
