<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <br>
    <p> First Name: <input v-model="params.first_name"></p>
    <p> Last Name: <input v-model="params.last_name"></p>
    <p> Email: <input v-model="params.email"></p>
    <p> Phone Number: <input v-model="params.phone_number"></p>
    <p> Image URL: <input v-model="params.image"></p>
    <button v-on:click="contactsCreate">Create</button>
    {{ params }}
    <hr>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <p>{{ contact.first_name }} {{ contact.last_name }}</p>
      <img v-bind:src="contact.image" style="width: 200px;">
    </div>
  </div>
</template>
 
<style>
p {
  font-weight: bold;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Albuquerque",
      contacts: [],
      params: {
        first_name: "",
        last_name: "",
        email: "",
        phone_number: "",
        image: ""
      }
    };
  },
  created: function() {
    this.contactsIndex();
  },
  methods: {
    contactsIndex: function() {
      console.log(this.contacts);
      axios
        .get("http://localhost:3000/api/contacts")
        .then(response => {
          this.contacts = response.data;
          console.log(response.data);
        });
    },
    contactsCreate: function () {
      console.log("creating...");
      axios
        .post("http://localhost:3000/api/contacts", this.params)
        .then(response => {
          this.contacts.push(response.data);
        });
    }
  }
};
</script>