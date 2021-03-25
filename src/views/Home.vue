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
    <br>
    <br>
    <br>
    <hr>
    <br>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <p>{{ contact.first_name }} {{ contact.last_name }} <button v-on:click="contactsShow(contact)">More info</button></p>
      
      <img v-bind:src="contact.image" style="width: 200px;">
      <br>
      <br>
      <br>
      <br>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <p>First Name: <input v-model="currentContact.first_name"></p>
        <p>Last Name: <input v-model="currentContact.last_name"></p>
        <p>Email: <input v-model="currentContact.email"></p>
        <p>Phone Number: <input v-model="currentContact.phone_number"></p>
        <p>Image URL: <input v-model="currentContact.image"></p>
        <button>Close</button>
        <button v-on:click="contactsUpdate">Update</button>
        <button v-on:click="contactsDestroy(currentContact)">Delete</button>
      </form>
    </dialog>
  </div>
</template>
 
<style>
p {
  font-weight: bold;
}

button {
  background-color: rgb(233, 221, 195);
}

dialog {
  text-align: left;
}

input {
  width: 400px;
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
      },
      currentContact: {}
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
    },
    contactsShow: function (contact) {
      console.log(contact);
      this.currentContact = contact
      document.querySelector("#contact-details").showModal();
    },
    contactsUpdate: function () {
      console.log("updating...");
      axios
        .patch("/api/contacts/" + this.currentContact.id, this.currentContact)
        .then(response => {
          console.log(response.data);
        })
    },
    contactsDestroy: function (theContact) {
      console.log("destroying...");
      axios
        .delete("/api/contacts/" + theContact.id)
        .then(response => {
          console.log(response.data);
          let index = this.contacts.indexOf(theContact);
          this.contacts.splice(index, 1);
        })
    }
  }
};
</script>