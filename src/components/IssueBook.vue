<template>
  <!-- <v-main>
    <v-container class="py-8 px-6" fluid> -->
  <v-snackbar v-model="snackbar" :color="color" :timeout="timeout" location="bottom">
      <div class="text-center">{{ message }}</div>
    </v-snackbar>
    <!-- <h1 class="thr">Issued Book List</h1> -->
      <div class="search-bar ms-5">
        <div class="search">
          <div class="search-bar">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-search icon" viewBox="0 0 16 16">
              <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001q.044.06.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1 1 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0"/>
            </svg>
            <input v-model="searchText" type="text" placeholder="Search" @input="search" />
          </div>
        </div>
        <input type="text" v-model="searchText" placeholder="Search by Name, Class, or Subject">
        <i class="fa fa-search" aria-hidden="true"></i> 
      </div>
      <div class="additional-inputs ms-5">
        <th style="background: #646c4c; color: white;" class="px-3 py-3">Teachers ID</th> <input type="text" placeholder="Teachers Id" v-model="teacherid">
        <th style="background: #646c4c; color: white;" class="px-3 py-3">Issued Book ID</th>
        <input type="text" placeholder="Book Id" v-model="bookid">
        <button @click="issueBook" style="background-color: #4397a0;">Issue Book</button>
      </div>
      <v-skeleton-loader
          v-if="loading"
          type="table-tbody"
        ></v-skeleton-loader>
        <v-table v-else class="table ms-5 mt-5 pe-5" height="450px" fixed-header>
      <thead>
        <tr class="bg-dark">
            <th>SlNo.</th>
            <th>Name</th>
            <th>Class</th>
            <th>Subject</th>
            <th>BookName</th>
            <th>IssuedDate</th>
            <th>ReturnDate</th>
            <th>Action</th>
        </tr>
      </thead>
      <tbody>
          <tr v-for="(teacher, index) in filteredTeachers" :key="index">
            <td>{{ index+1 }} {{ teacher.userId }}</td>
            <!-- <td>{{ teacher.id }}</td> -->
            <td><input type="text" v-model="teacher.teacherName" :disabled="!teacher.editMode" /></td>
          <td><input type="text" v-model="teacher.class" :disabled="!teacher.editMode" /></td>
          <td><input type="text" v-model="teacher.subject" :disabled="!teacher.editMode" /></td>
          <td><input type="text" v-model="teacher.bookName" :disabled="!teacher.editMode" /></td>
          <td><input type="text" v-model="teacher.issueDate" :disabled="!teacher.editMode" /></td>
          <td><input type="text" v-model="teacher.returnDate" :disabled="!teacher.editMode" /></td>
          <td>
            <button class="btn" style="background-color: #4397a0; color:white" @click="returnBook(index)">Return</button>
          </td>
          </tr>
        </tbody>
    </v-table>
      <!-- <table  class="ms-5 mt-5">
        <thead>
          <tr>
          <th>SlNo.</th>
            <th>Name</th>
            <th>Class</th>
            <th>Subject</th>
            <th>BookName</th>
            <th>IssuedDate</th>
            <th>ReturnDate</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(teacher, index) in filteredTeachers" :key="index">
            <td>{{ index+1 }} {{ teacher.userId }}</td>
            <td>
              <input type="text" v-model="teacher.teacherName" :disabled="!teacher.editMode">
            </td>
            <td>
              <input type="text" v-model="teacher.class" :disabled="!teacher.editMode">
            </td>
            <td>
              <input type="text" v-model="teacher.subject" :disabled="!teacher.editMode">
            </td>
            <td>
              <input type="text" v-model="teacher.bookName" :disabled="!teacher.editMode">
            </td>
            <td>
              <input type="text" v-model="teacher.issueDate" :disabled="!teacher.editMode">
            </td>
            <td>
              <input type="text" v-model="teacher.returnDate" :disabled="!teacher.editMode">
            </td>
            <td>
              <button class="btn" style="background-color: #4397a0; color:white" @click="returnBook(index)">Return</button>
            </td>
          </tr>
        </tbody>
      </table> -->
    <!-- </v-container>
    </v-main> -->
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      teachers: [
    {
        bookId: 8,
        bookName: "Chemmeen",
        issueDate: "2024-06-13",
        returnDate: "2024-07-11",
        teacherId: 5,
        teacherName: "Sangeetha",
        class: "6",
        division: "C",
        subject: "Hindi",
        quantity: 1
    },
    {
        bookId: 10,
        bookName: "Pathummayude Aadu",
        issueDate: "2024-06-16",
        returnDate: "2024-07-14",
        teacherId: 2,
        teacherName: "Stephy",
        class: "8",
        division: "A",
        subject: "Mathematics",
        quantity: 2
    },
    {
        bookId: 10,
        bookName: "Pathummayude Aadu",
        issueDate: "2024-06-16",
        returnDate: "2024-07-14",
        teacherId: 2,
        teacherName: "Stephy",
        class: "8",
        division: "A",
        subject: "Mathematics",
        quantity: 2
    }
      ],
      searchText: '',
      bookid: null,
      teacherid: null,
      snackbar: false,
      color: '#E8F5E9',
      timeout: 3000,
      message: '',
      loading: true,
    };
  },
  computed: {
    filteredTeachers() {
      if(this.searchText !== '')
      {
        return this.teachers.filter(teacher =>
        teacher.teacherName.toLowerCase().includes(this.searchText.toLowerCase()) ||
        teacher.class.toLowerCase().includes(this.searchText.toLowerCase()) ||
        teacher.subject.toLowerCase().includes(this.searchText.toLowerCase()) ||
        teacher.bookName.toLowerCase().includes(this.searchText.toLowerCase()) 
      );
      } else
      return this.teachers; 
    }
  },
  mounted(){
this.getissuedbooks()
  },
  methods: {
    async issueBook() {
    try {
        const response = await axios.post(`${this.$store.getters.getUrl}/issues/issue-book?bookId=${this.bookid}&userId=${this.teacherid}`, {
            "bookId": this.bookid,
            "userId": this.teacherid 
        });
        if (response.status >= 200 || response.status < 300) {
            // alert('Successfully issued');
            this.message = 'Book Issued Successfully !!';
            this.color = 'green';
            this.clearForm();
            this.snackbar = true;
            this.getissuedbooks()
        }
    } catch (error) {
        this.message = error.response.data + '!!';
        this.color = 'red';
        this.snackbar = true;
        console.error(error);
        // alert(error.response.data)
    }
},
clearForm() {
      this.teacherid = '';
      this.bookid = '';
    },
   async getissuedbooks(){
    this.loading = false;
try{
  const response = await axios.get(`${this.$store.getters.getUrl}/issues/allissuedbooks`)
  if(response.status >= 200 || response.status < 300){
    this.teachers = response.data
  
    console.log(response.data)
  }                                                                                             
}
catch(error){
  console.error(error)
}
   },
   async returnBook(index) {
    try {
        const bookid=this.filteredTeachers[index].bookId
        console.log(bookid);
        console.log(typeof(bookid));
        const teacherid=this.filteredTeachers[index].teacherId
        const response = await axios.post(`${this.$store.getters.getUrl}/return/return-book?bookId=${bookid}&userId=${teacherid}`, {
          "bookId": this.bookid,
          "userId": this.teacherid 
        });
        if (response.status >= 200 || response.status < 300) {
          this.message = 'Book Returned Successfully !!';
          this.color = 'green';
          this.snackbar = true;
            // alert('Returned Successfully');
          this.getissuedbooks()
        }
    } catch (error) {
        console.error(error);
        alert(error)
    }
},
    search() {
      // Function to handle search
      // Since we are using v-model, searchText is automatically updated
    }
  }
};
</script>

<style scoped>
  /* Your existing CSS styles */
  /* table {
    width: 96%;
    border-collapse: collapse;
  }
  
  th, td {
    border: 1px solid #ddd;
    padding: 8px;
  }
  
  th {
    background-color: #f2f2f2;
  }
  
  tbody tr:nth-child(even) {
    background-color: #f2f2f2;
  } */
  
  /* table input[type="text"] {
    border: none; 
    outline: none; 
    width: 100%; 
    padding: 4px; 
    box-sizing: border-box; 
  } */
  
  /* .thr {
    text-align: center;
    margin-top: 10px;
    margin-bottom: 2px;
    padding: 8px;
  }
  
  table button {
    border: none;
    cursor: pointer;
    padding: 10;
  } */
  .navbar {
    background-color: #f4c807;
    color: #150101;
    padding: 5px 0;
  }
  
  .container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: 1200px;
    margin: 0 auto;
  }
  .navbar-brand {
    font-size: 1.5rem;
    text-decoration: white;
    color: #fff;
  }
  
  .navbar-nav {
    list-style-type: none;
    margin: 0px;
    padding: 0px;
    display: flex;
  }
  
  .nav-item {
    margin-right: 20px;
  }
  
  .nav-link {
    text-decoration:white;
    color: #fff;
  }
  
  .search-bar {
    width: 250px;
    border: none; /* Remove the border */

  }
  
  .search-bar input[type="text"] {
    border: none; /* Remove border */
    outline: none; /* Remove outline */
    width: 100%; /* Full width */
    padding: 4px; /* Add padding */
    box-sizing: border-box; /* Include padding and border in the element's total width and height */
  }
  
  /* Rest of your CSS */
  
  .search-bar {
    display: flex;
    align-items: center;
    width: 250px;
    border: none; /* Remove the border */
    border-bottom: 1px solid black; /* Add bottom border */
  }
  
  .search-bar input[type="text"] {
    border: none; /* Remove border */
    outline: none; /* Remove outline */
    width: calc(100% - 30px); /* Full width minus space for icon */
    padding: 4px; /* Add padding */
    box-sizing: border-box; /* Include padding and border in the element's total width and height */
  }
  
  .search-bar i {
    margin-left: 5px; /* Adjust icon spacing */
  }
  
.additional-inputs {
  margin-top: 75px; /* Adjust the margin as needed */
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 100%; /* Adjust the width as needed */
  /* margin-left: 250px; */
}

.additional-inputs input[type="text"] {
  width: calc(33.33% - 10px); /* Divide the width equally between inputs with some margin */
  padding: 8px; /* Add padding */
  box-sizing: border-box; /* Include padding and border in the element's total width and height */
}

.additional-inputs button {
  padding: 8px 16px; /* Add padding */
  background-color: #007bff; /* Change button background color */
  color: #fff; /* Change button text color */
  border: none; /* Remove border */
  border-radius: 4px; /* Add border radius */
  cursor: pointer;
}

.additional-inputs button:hover {
  background-color: #0056b3; /* Change button background color on hover */
}

.v-table.v-table--fixed-header > .v-table__wrapper > table > thead > tr > th {
  background: #646c4c;
  color: white;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.table th,
.table td {
  border: 1px solid #ddd; /* Border style and color */
  padding: 8px;
}

.table th {
  background-color: #f2f2f2;
}

table input[type="text"] {
  border: none; 
  outline: none; 
  width: 100%; 
  padding: 4px; 
  box-sizing: border-box; 
}

.v-table.v-table--fixed-header > .v-table__wrapper > table > thead > tr > th {
  background: #646c4c;
  color: white;
}
  </style>