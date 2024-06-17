<template>
  <v-dialog width="450" v-model="dialog">
    <template v-slot:activator="{ props }">
      <v-btn v-bind="props" text="Sign Up" class="btn buttonstyle text-white"></v-btn>
    </template>
    <template v-slot:default="{ isActive }">
      <v-card-actions>
        <v-icon class="mdi mdi-close text-white" @click="isActive.value = false"></v-icon>
      </v-card-actions>
      <v-card title="Registration for teachers" class="text-center">
        <v-card-text>
          <form @submit.prevent="register">
            <div class="my-3">
              <input type="text" class="form-control borderstyle" v-model="name" placeholder="Enter name">
              <span v-if="nameError" class="error">{{ nameError }}</span>
            </div>
            <div class="my-3">
              <input type="email" class="form-control borderstyle" v-model="email" placeholder="Enter email">
              <span v-if="emailError" class="error">{{ emailError }}</span>
            </div>
            <div class="my-3">
              <input type="text" class="form-control borderstyle" v-model="std" placeholder="Enter your class">
              <span v-if="stdError" class="error">{{ stdError }}</span>
            </div>
            <div class="my-3">
              <input type="text" class="form-control borderstyle" v-model="div" placeholder="Enter your division">
              <span v-if="divError" class="error">{{ divError }}</span>
            </div>
            <div class="my-3">
              <input type="text" class="form-control borderstyle" v-model="sub" placeholder="Enter your subject">
              <span v-if="subError" class="error">{{ subError }}</span>
            </div>
            <div class="my-3 password-container">
              <input :type="passwordFieldType" v-model="password" class="form-control borderstyle" placeholder="Password">
              <span class="password-toggle" @click="togglePasswordVisibility">
                <v-icon :class="passwordFieldType === 'password' ? 'mdi mdi-eye-off' : 'mdi mdi-eye'"></v-icon>
              </span>
              <span v-if="passwordError" class="error">{{ passwordError }}</span>
            </div>
            <v-btn type="submit" class="btn buttonstyle text-white mt-3 mb-4" color="#303b1b">Sign Up</v-btn>
          </form>
        </v-card-text>
      </v-card>
    </template>
  </v-dialog>
  <v-dialog v-model="dialog2">
    <v-card class="mx-auto rounded-5" elevation="2" max-width="400" width="100%" height="300">
      <v-card-title class="green-header bg-green py-3"></v-card-title>
      <v-card-text class="text-success text-center">
        <v-icon class="mb-2 mt-2" color="success" icon="mdi-check-circle" size="100"></v-icon>
        <h2>Successfully Registered</h2>
        <p>Get the app from Play Store</p>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      name: '',
      email: '',
      std: '',
      div: '',
      sub: '',
      password: '',
      nameError: '',
      emailError: '',
      stdError: '',
      divError: '',
      subError: '',
      passwordError: '',
      dialog: false,
      dialog2: false,
      passwordFieldType: 'password'
    };
  },
  methods: {
    togglePasswordVisibility() {
      this.passwordFieldType = this.passwordFieldType === 'password' ? 'text' : 'password';
    },
    validate() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      this.emailError = !emailRegex.test(this.email) ? 'Enter a valid email address' : '';

      // Example: validate name
      this.nameError = this.name.trim() === '' ? 'Name is required' : '';

      // Example: validate std
      const stdValue = parseInt(this.std);
      this.stdError = isNaN(stdValue) || stdValue < 0 || stdValue > 12 ? 'Class must be a number between 0 and 12' : '';

      // Example: validate div
      this.divError = this.div.trim() === '' ? 'Division is required' : '';

      // Example: validate sub
      this.subError = this.sub.trim() === '' ? 'Subject is required' : '';

      const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/;
      this.passwordError = !passwordRegex.test(this.password) ?
        'Password must be at least 8 characters long and include at least one uppercase letter, one lowercase letter, and one number' : '';

      // Check if all errors are empty
      return (
        this.nameError === '' &&
        this.emailError === '' &&
        this.stdError === '' &&
        this.divError === '' &&
        this.subError === '' &&
        this.passwordError === ''
      );
    },
    async register() {
      if (this.validate()) {
        try {
          const response = await axios.post(`${this.$store.getters.getUrl}/api/REG`, {
            "teacherName": this.name,
            "password": this.password,
            "email": this.email,
            "standard": this.std,
            "division": this.div,
            "subject": this.sub,
            "role": 'teacher'
          });
          if (response.status >= 200 || response.status < 300) {
            this.dialog2 = true; 
            this.dialog = !this.dialog;
            this.clearForm();
          }
        } catch (error) {
          console.error(error)
        }
      }
    },
    clearForm() {
      this.name = '';
      this.email = '';
      this.std = '';
      this.div = '';
      this.sub = '';
      this.password = '';
    },
  }
};
</script>

<style scoped>
.log-in input {
  width: 300px;
  height: 40px;
  padding-left: 20px;
  display: block;
  margin-bottom: 10px; /* Reduced margin between input and error */
  margin-right: auto;
}
.error {
  color: rgb(235, 40, 40);
}
.btn {
  background-color: rgb(117, 184, 211);
}
.btn:hover {
  background: rgb(16, 100, 133);
}
.buttonstyle {
  background-color: #303b1b;
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
