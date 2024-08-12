<template>
  <logout-button-component v-if="this.token"
      @user_logout="logoutUser"
  />
  <login-form-component v-if="!this.token"
      @user_login="loginUser"
  />
  <sign-up-form-component v-if="!this.token"
      @user_signup="signUpUser"
  />
  <search-bar-component v-if="this.token"
      @send_search_query="searchOuter"
  />
  <outer-addresses-list-component v-if="this.token"
      :outerAddresses="outerAddresses"
      :currentUserId="currentUserId"
      @address_save="saveAddress"
  />
  <saved-addresses-component v-if="this.token"
      :savedAddresses="savedAddresses"
  />
</template>

<script>
import axios from 'axios'
import SignUpFormComponent from "@/components/SignUpFormComponent";
import LoginFormComponent from "@/components/LoginFormComponent";
import LogoutButtonComponent from "@/components/LogoutButtonComponent";
import SavedAddressesComponent from "@/components/SavedAddressesComponent";
import SearchBarComponent from "@/components/SearchBarComponent";
import OuterAddressesListComponent from "@/components/OuterAddressesListComponent";
export default {
  name: "App",
  components: {
    SignUpFormComponent, LoginFormComponent, LogoutButtonComponent,
    SavedAddressesComponent, SearchBarComponent, OuterAddressesListComponent,
  },
  data() {
    return {
      api: {},
      token: null,
      dadataToken : 'e4f976755769e2476df6c7db744f8813b2e7fbec',
      dadataSecret: '099e2c7f7273185a6114294a839457c348c02319',
      currentUserId: null,
      savedAddresses: [],
      outerAddresses: [],
    }
  },
  methods: {
    saveAddress(newAddress) {
      console.log(newAddress)
      try {
        const response = this.api.post('/save', newAddress, {
          headers: {
            'Authorization': `Bearer ${this.token}`,
            'Content-Type': 'application/json',
          }
        })
            .then(response => {
              console.log(response.data)
            });
      } catch (error) {
        console.log(error.response.data.message)
      }
    },
    searchOuter(searchQuery) {
      let query = {
        query: searchQuery
      };
      if(searchQuery === '') {
        this.outerAddresses = [];
      }
      else {
        try {
          const response = this.api.post('/search', query, {
            headers: {
              'Content-Type': 'application/json',
              'Authorization': `Bearer ${this.token}`,
            }})
              .then(response => {
                console.log(response.data);
                this.outerAddresses = response.data.suggestions;
              });
        } catch (error) {
          console.log(error.response.data.message)
        }
      }
    },
    loginUser(loginData) {
      try {
        const response = this.api.post('/login', loginData)
            .then(response => {
            sessionStorage.setItem('token', response.data.token);
            sessionStorage.setItem('currentUserId', response.data.user.id);
            this.token = sessionStorage.getItem('token');
            this.currentUserId = sessionStorage.getItem('currentUserId');
              console.log(response.data)
        });
      } catch (response) {
        console.log(response)
      }
    },
    signUpUser(SignUpData) {
      try {
        const response = this.api.post('/signup', SignUpData)
        .then(response => {
          sessionStorage.setItem('token', response.data.token);
          sessionStorage.setItem('currentUserId', response.data.user.id);
          this.token = sessionStorage.getItem('token');
          this.currentUserId = sessionStorage.getItem('currentUserId');
          console.log(response.data)
        });
      } catch (error) {
        console.log(error.response.data.message)
      }
    },
    logoutUser() {
              sessionStorage.removeItem('token');
              sessionStorage.removeItem('currentUserId');
              this.token = null;
              this.currentUserId = null;
    }
  },
  mounted() {
    this.token = sessionStorage.getItem('token');
    this.currentUserId = sessionStorage.getItem('currentUserId');

    this.api = axios.create({
      withCredentials: true,
      baseURL: 'http://localhost:8000/api',
      headers: {
        'Accept': 'application/json',
        'Access-Control-Allow-Origin': 'http://localhost:8000',
      }
    });

    axios.get('http://localhost:8000/sanctum/csrf-cookie', {
      headers : {
        'Content-Type': 'application/json',
      }
    })

    this.api.get('/index', {
      headers: {
        'Authorization': `Bearer ${this.token}`
      }})
        .then(response => {
          this.savedAddresses = response.data
        })
        .catch(error => {
          console.log(error.response.data.message)
        });
  }
}
</script>

<style scoped>
.form {
  border: 2px solid black;
}
</style>