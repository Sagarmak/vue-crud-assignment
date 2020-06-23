<template>
  <div class="home-component container">
    <div class="header flex justify-between align-center">
      <div class="title">Employees</div>
      <div class="button" @click="createEmployee"><button>Create</button></div>
    </div>
    <div class="mini-header flex justify-between align-center">
      <input v-model="searchedName" type="text" name="" placeholder="Search Name">
      <!-- <div class="result">
        {{ searchedResult }}
      </div> -->
      <div class="sort">
        Sort: Name:
        <select v-model="sortBy" name="type">
          <option value="normal" selected>Normal</option>
          <option value="asc">Ascending</option>
          <option value="desc">Descending</option>
        </select>
      </div>
    </div>
    <div class="body">
      <table>
        <tr>
          <th v-for="header in tableHeaders" :key="header.value" class="bold">{{ header.text }}</th>
        </tr>
        <tr v-for="data in searchedResult" :key="data.id">
            <td @click="showEmployee(data)">{{ data.id }}</td>
            <td>{{ data.preferredFullName }}</td>
            <td>{{ data.employeeCode }}</td>
            <td>{{ data.jobTitleName }}</td>
            <td>{{ data.phoneNumber }}</td>
            <td>{{ data.emailAddress }}</td>
            <td>{{ data.region }}</td>
            <td>{{ data.dob }}</td>
            <td>
              <button class="mr-2" @click="deleteAnEmployee(data.id)"><i class="fas fa-times-circle"></i></button>
              <button @click="updateAnEmployee(data)"><i class="fas fa-edit"></i></button>
            </td>
        </tr>
      </table>

      <ViewEmployee 
        v-if="showViewEmployeeModal"
        :data="employeeData"
        @view-employee="viewEmployeeActionHandler" 
      />
      <CreateEmployee 
        v-if="showAndUpdateCreateEmployeeModal"
        :data="employeeData"
        @create-employee="createEmployeeActionHandler" 
      />

    </div>

    <div class="footer flex justify-between align-center">
      <button :disabled="disablePrevious" @click="previousPage">Prev</button>
      <div class="text">
        Page {{ currentPage }} of {{ totalPages }}
      </div>
      <button :disabled="disableNext" @click="nextPage">Next</button>
    </div>
  </div>
</template>

<script>
import ViewEmployee from './ViewEmployee';
import CreateEmployee from './CreateEmployee';

export default {
  name: 'HomeComponent',
  created() {
    this.getAllEmployees();
  },
  components: {
    ViewEmployee,
    CreateEmployee
  },
  data() {
    return {
      searchedName: '',
      sortBy: '',
      allEmployees: [],
      tableHeaders: [
        { text: 'ID', value: 'id' },
        { text: 'Full Name', value: 'preferredFullName' },
        { text: 'Employee Code', value: 'employeeCode' },
        { text: 'Job Title', value: 'jobTitleName' },
        { text: 'Phone No', value: 'phoneNumber' },
        { text: 'Email ID', value: 'emailAddress' },
        { text: 'Region', value: 'region' },
        { text: 'DOB', value: 'dob' },
        { text: 'Actions', value: 'actions' }
      ],
      employeeData: {},
      showViewEmployeeModal: false,
      showAndUpdateCreateEmployeeModal: false,

      // pagination
      // allEmployees,
      employeesToDisplay: [],
      perPage: 10,
      currentPage: 1
    };
  },
  methods: {
    getAllEmployees() {
      const url = 'http://localhost:3000/employees';
      fetch(url)
      .then((response) => response.json())
      .then((data) => {
        this.allEmployees = data;
        this.paginateData();
      })
      .catch((err) => {
        console.log(err);
      });
    },
    paginateData() {
      this.employeesToDisplay = [];

      const lastPageInDecimal = this.allEmployees.length / this.perPage;
      const lastPage = Math.ceil(lastPageInDecimal);
      
      let last = this.currentPage * this.perPage;
      let first = last - this.perPage;

      if(lastPage == this.currentPage) {
        last = this.allEmployees.length;
      }

      for (let index = first; index < last; index++) {
        this.employeesToDisplay.push(this.allEmployees[index]);
      }

      // result -> this.employeesToDisplay
    },
    createEmployee() {
      this.employeeData = null;
      this.showAndUpdateCreateEmployeeModal = true; 
    },
    deleteAnEmployee(id) {
      const url = `http://localhost:3000/employees/${id}`;
      const params = {
        method: 'DELETE'
      };
      
      fetch(url, params)
      .then((response) => {
        this.getAllEmployees();
      })
      .catch((err) => {
        console.log(err);
      });
    },
    showEmployee(data) {
      this.employeeData = data;
      this.showViewEmployeeModal = true;
    },
    viewEmployeeActionHandler(data) {
      if (!data) {
        this.showViewEmployeeModal = false;
      }
    },
    createEmployeeActionHandler(data) {
      if (!data) {
        this.showAndUpdateCreateEmployeeModal = false;
      }
      // console success message, fetch the details again and close the modal
      this.showAndUpdateCreateEmployeeModal = false;
      this.getAllEmployees();
    },
    updateAnEmployee(data) {
      this.employeeData = data;
      this.showAndUpdateCreateEmployeeModal = true;
    },
    sort(data, type) {
      if (type == 'asc') {
        return data.sort((a, b) => {
          let nameA = a.preferredFullName.toLowerCase();
          let nameB = b.preferredFullName.toLowerCase();
          
          if (nameA < nameB) {
              return -1;
          }
          if (nameA > nameB) {
              return 1;
          }
          return 0;
        });
      } else {
        return data.sort((a, b) => {
          let nameA = a.preferredFullName.toLowerCase();
          let nameB = b.preferredFullName.toLowerCase();
          
          if (nameA > nameB) {
              return -1;
          }
          if (nameA < nameB) {
              return 1;
          }
          return 0;
        });
      }
    },
    nextPage() {
      this.currentPage++;
      this.paginateData();
    },
    previousPage() {
      this.currentPage--;
      this.paginateData();
    }
  },
  computed: {
    searchedResult() {
      let data = this.employeesToDisplay.filter((item) => item.preferredFullName.toLowerCase().includes(this.searchedName));

      if (this.sortBy == 'asc' || this.sortBy == 'desc') {
        return this.sort(data, this.sortBy);
      } else {
        return data;
      }
    },
    totalPages() {
      const lastPageInDecimal = this.allEmployees.length / this.perPage;
      const lastPage = Math.ceil(lastPageInDecimal);
      return lastPage;
    },
    disableNext() {
      return this.currentPage == this.totalPages;
    },
    disablePrevious() {
      return this.currentPage == 1;
    }
  }
}
</script>

<style>
.home-component {
  /*  */
}
.home-component .header, .home-component .mini-header {
  padding-bottom: 1rem;
}
.home-component .header .button button {
  color: white;
  background: blue;
  padding: 0.5rem 1rem;
  border-radius: 5px;
}
.home-component .body {
  /*  */
}
.home-component .body table {
  border-collapse: collapse;
  width: 100%;
}
.home-component .body table th, td {
  padding: 5px;
  border: solid 1px black;
}
.home-component .body table tr:hover {
  background: #ececec;
}
.home-component .footer {
  margin-top: 1rem;
}
</style>