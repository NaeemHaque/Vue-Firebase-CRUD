<template>
  <div id="app" class="container bg-secondary">
    <div class="raw">
      <nav class="navbar navbar-light">
        <h3>Vue CRUD</h3>
      </nav>
    </div>

    <div class="raw">
      <form class="col-lg-6 col-md-8 col-sm-10 col-xs-6 m-auto">
        <div class="mb-3">
          <label class="form-label text-light">Username</label>
          <input
            type="text"
            class="form-control"
            placeholder="Your Username Here"
            v-model="user.userName"
          />
        </div>

        <div class="mb-3">
          <label for="exampleInputEmail1" class="form-label text-light"
            >Email address</label
          >
          <input
            type="email"
            class="form-control"
            aria-describedby="emailHelp"
            placeholder="example@example.com"
            v-model="user.email"
          />
        </div>

        <div class="text-center mb-3">
          <button type="submit" class="btn btn-primary" @click.prevent="addUser">
            Add
          </button>
        </div>
      </form>
    </div>

    <div class="raw">
      <table class="table table-success table-striped">
        <table class="table text-center">
          <thead>
            <tr>
              <th scope="col">UserName</th>
              <th scope="col">Email</th>
              <th scope="col">Edit</th>
              <th scope="col">Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="userInfo in userData" :key="userInfo">
              <td>
                {{userInfo.val.userName}}
              </td>
              <td>{{userInfo.val.email}}</td>
              <td>
                <button type="button" class="btn btn-info p-2">Edit</button>
              </td>
              <td>
                <button type="button" class="btn btn-danger p-2" @click.prevent="deleteUser(userInfo.key)">Delete</button>
              </td>
            </tr>

          </tbody>
        </table>
      </table>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase'


export default {
  data() {
    return {
      user: {
        userName: "",
        email: ""
      },
      dbRef: null,
      userData: []
    };
  },
  methods: {
    addUser() {
      // console.log(this.user);
      this.dbRef.push(this.user);

      this.user = {   
        userName: "",
        email: ""
      }

    },

    deleteUser(userKey){
      // console.log(userKey);
      this.dbRef.child(userKey).remove();
    }
  },
  mounted() {
    console.log("mounted");

  var firebaseConfig = {
    apiKey: "AIzaSyDRky4CgVlazX7DG1-sUrODoAtFDMLFjdU",
    authDomain: "vue-crud-firebase-a3d4b.firebaseapp.com",
    databaseURL: 'https://vue-crud-firebase-a3d4b-default-rtdb.firebaseio.com/',
    projectId: "vue-crud-firebase-a3d4b",
    storageBucket: "vue-crud-firebase-a3d4b.appspot.com",
    messagingSenderId: "801574455616",
    appId: "1:801574455616:web:680f9967772474c9cf92a5"
  };
  // Initialize Firebase
  let firebaseApp = firebase.initializeApp(firebaseConfig);
  //create node "users" on database
  this.dbRef = firebaseApp.database().ref('users');

  const ci = this;
  this.dbRef.on('value', snapshot => {
    ci.userData = [];
    snapshot.forEach(element => {
      // console.log(element.val());
      ci.userData.push(
        {
          key: element.key, 
          val: element.val()
        })
    });
  })
  },
};
</script>

<style>
.navbar {
  background-color: #C3E6CB;
}
</style>
