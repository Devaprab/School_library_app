<template>
  <v-main>
      <v-container justify-center>
        <v-snackbar v-model="snackbar" :color="color" :timeout="timeout" location="top">
      <div class="text-center">{{ message }}</div>
    </v-snackbar>
          <section class="">
<div class="container-fluid mt-5 pt-5">
  <div class="row d-flex justify-content-start align-items-center">
    <div class="col-md-9 col-lg-6 col-xl-5">
      <v-img src="../assets/Login book.png"
        class="img-fluid"></v-img>
    </div>
    <div class="col-md-8 col-lg-6 col-xl-4 offset-xl-1">
      <v-form ref="form" @submit.prevent="register">
        <!-- Name input -->
        <div data-mdb-input-init class="form-outline mb-4">
            <v-text-field label="Enter Your Usrname" v-model="adminname" variant="solo" class="custom-text-field" :rules="nameRules"></v-text-field>
        </div>
        <!-- Email input -->
        <div data-mdb-input-init class="form-outline mb-4">
            <v-text-field label="Enter Your Email" v-model="email" variant="solo" class="custom-text-field" :rules="emailRules"></v-text-field>
        </div>
        <!-- Password input -->
        <div data-mdb-input-init class="form-outline mb-3">
          <v-text-field v-model="pswd" :append-inner-icon="visible ? 'mdi-eye' : 'mdi-eye-off'"
            :type="visible ? 'text' : 'password'"   label="Password"
             prepend-inner-icon="mdi-lock" :rules="passRules"
            @click:append-inner="visible = !visible">
          </v-text-field>
        </div>

        <div class="text-center text-lg-start mt-4 pt-2">
          <v-btn type="submit"
            style=" background-color: #007bff; color: white;">Register</v-btn>
          <p class="small fw-bold mt-2 pt-1 mb-0">Already have an account? <router-link to="/login"
              >Login</router-link></p>
        </div>

      </v-form>
    </div>
  </div>
</div>
</section>
      </v-container>
  </v-main>
</template>

<script>
  import axios from 'axios';
  export default {
    data() {
      return {
        adminname: '',
        pswd: '',
        email: '',
        dialog: false,
        visible: false,
        snackbar: false,
      color: '#E8F5E9',
      timeout: 3000,
      message: '',
        nameRules: [
      value => !!value || 'Name is required.',
      value => /^[^\s\W]/.test(value) || 'Name should not start with a special character.',
      value => /^\D+$/.test(value) || 'Name should not contain digits.',
      value => (value?.length >= 3) || 'Name must contain at least 3 characters.',
      value => !/[^a-zA-Z\s.]/g.test(value) || 'Name should not contain special characters',
    ],
    emailRules: [
      value => !!value || 'E-mail is required.',
      value => /.+@.+\..+/.test(value) || 'E-mail must be valid.',
    ],
    passRules: [
    value => !!value || 'Password is required.',
      value => /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*()_+}{":;'?/>.<,])[a-zA-Z\d!@#$%^&*()_+}{":;'?/>.<,]{8,}$/.test(value) ||  'Password must contain at least 8 characters, including uppercase, lowercase letters, special character and a digit',

    ],
      }
    },
    methods: {
      async register() {
        console.log("clicked register");
        const { valid } = await this.$refs.form.validate();
        if (valid) {
        try {
          const response = await axios.post(`${this.$store.getters.getUrl}/admin/AdminReg`, {
            "adminName": this.adminname,
            "password": this.pswd,
            "email": this.email,
            "role": 'admin'
          });
          if (response.status >= 200 || response.status < 300) {
            this.message = 'Successfully registered please Login !!';
            this.color = 'green';
            this.clearForm();
            this.snackbar = true;
          }
        } catch (error) {
          this.message = error.response.data + '!!';
          this.color = 'red';
          this.snackbar = true;
          console.error(error)
        }
      }
      },
      clearForm() {
      this.adminname = '';
      this.pswd = '';
      this.email = '';
    },
    }
  }
  </script>

<style scoped>
:deep(.v-input__control ) {
background-color: white; /* Set the input text color to white */
}
</style>