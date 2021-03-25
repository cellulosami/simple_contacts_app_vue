<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <br>
    <div>
      <p> First Name:  <input v-model="params.first_name"></p>
      <p> Last Name:  <input v-model="params.last_name"></p>
      <p> Email:  <input v-model="params.email"></p>
      <p> Phone Number:  <input v-model="params.phone_number"></p>
      <p> Image URL:  <input v-model="params.image"></p>
    </div>
    <button v-on:click="contactsCreate">Create</button>
    <br>
    <br>
    <br>
    <hr>
    <br>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <p>{{ contact.first_name }} {{ contact.last_name }} <button v-on:click="contactsShow(contact)">More info</button></p>
      
      <img v-bind:src="contact.image" alt="Image failed to load" style="width: 200px;">
      <br>
      <br>
      <br>
      <br>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <p>First Name:<input v-model="currentContact.first_name"></p>
        <p>Last Name:<input v-model="currentContact.last_name"></p>
        <p>Email:<input v-model="currentContact.email"></p>
        <p>Phone Number:<input v-model="currentContact.phone_number"></p>
        <p>Image URL:<input v-model="currentContact.image"></p>
        <button>Close</button>
        <button v-on:click="contactsUpdate">Update</button>
        <button v-on:click="contactsDestroy(currentContact)">Delete</button>
      </form>
    </dialog>
  </div>
</template>
 
<style>
body {
  background-color: rgb(250, 248, 242);
  font-weight: bold;
}

button {
  background-color: rgb(233, 221, 195);
  transition: ease 10s;
}

dialog {
  text-align: end;
}

input {
  float: center;
  background-image: linear-gradient(to top, white 40%, rgb(221, 221, 221));
  border: 1px solid rgb(101, 101, 101);
  width: 400px;
  padding: 3px 4px;
  margin-left: 8px;
  margin-right: 100px;
}

input:hover,
input:focus {
  color: rgb(72, 72, 72);
  cursor: wait;
}

body:hover,
body:focus {
  cursor: wait;
}

button:hover,
button:focus {
  cursor: pointer;
  margin-left: 1000px;
  margin-right: 1000px;
  transition: ease 10s;
}

img {
  transition: ease 10s;
}

img:hover {
  opacity: 0;
  margin-left: 1000px;
  transition: ease 0.5s;
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