<template>
  <v-main>
    <v-container justify-center>
  <div class="mb-4 pb-2 mt-5 d-flex justify-content-center">
    <form class="form-signin mb-5 pb-4" @submit.prevent="login">
      <h1 class="h3 mb-3 font-weight-normal mt-5">Please Log In</h1>
      <label for="inputuser" class="sr-only">Email</label>
      <input type="email" id="inputuser" class="form-control" placeholder="Email" v-model="email">
      <p v-if="emailError" class="error-message">{{ emailError }}</p>
      <label for="inputPassword" class="sr-only">Password</label>
      <div class="password-container">
        <input :type="passwordFieldType" id="inputPassword" class="form-control" placeholder="Password" v-model="pswd">
        <span class="password-toggle" @click="togglePasswordVisibility">
          <v-icon :class="passwordFieldType === 'password' ? 'mdi mdi-eye-off' : 'mdi mdi-eye'"></v-icon>
        </span>
      </div>
      <span v-if="passwordError" class="error-message">{{ passwordError }}</span>
      <div class="checkbox mb-3"></div>
      <button class="btn btn-lg buttonstyle btn-block text-white" type="submit">Log In</button>
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
      <p class="mt-3">Are you a new admin?<router-link to="/adminregister">Register</router-link></p>
    </form>
  </div>
  </v-container>
  </v-main>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      pswd: '',
      email: '',
      emailError: '',
      passwordError: '',
      passwordFieldType: 'password'
    }
  },
  methods: {
    togglePasswordVisibility() {
      this.passwordFieldType = this.passwordFieldType === 'password' ? 'text' : 'password';
    },
    async login() {
      this.emailError = '';
      this.passwordError = '';
      if (!this.email || !this.pswd) {
        if (!this.email) this.emailError = 'Email is required';
        if (!this.pswd) this.passwordError = 'Password is required';
        return;
      }

      // Email validation
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailRegex.test(this.email)) {
        this.emailError = 'Invalid email address';
        return;
      }

      // Password validation
      const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*()_+}{":;'?/>.<,])[a-zA-Z\d!@#$%^&*()_+}{":;'?/>.<,]{8,}$/;
      if (!passwordRegex.test(this.pswd)) {
        this.passwordError = 'Password must be at least 8 characters long and contain at least one uppercase letter, one lowercase letter, special character and one number';
        return;
      }

      try {
        this.emailError = '';
        this.passwordError = '';
        const response = await axios.post(`${this.$store.getters.getUrl}/admin/AdminLogin`, {
          "email": this.email,
          "password": this.pswd,
        });
        if (response.status >= 200 || response.status < 300) {
          this.$router.push('/adminpage');
        }
      } catch (error) {
        console.error(error);
      }
    }
  }
}
</script>

<style scoped>
.buttonstyle {
  background-color: #303b1b;
}

.buttonstyle:hover {
  background-color: green;
}

form.form-signin {
  border-radius: 10px;
  padding-left: 50px;
  padding-right: 50px;
  background-color: white;
  padding-top: 20px;
  margin-left: 50px;
  margin-right: 50px;
  width: 350px;
}

.error-message {
  color: red;
  font-size: 14px;
}

.password-container {
  position: relative;
}

.password-toggle {
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  cursor: pointer;
}

.password-toggle i {
  font-size: 18px;
}
</style>
