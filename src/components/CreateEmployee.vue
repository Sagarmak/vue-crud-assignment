<template>
  <div class="create-employee">
    <div class="modal">
      <div class="modal-content">
        <div class="header flex align-items justify-between">
          <div class="title bold">Create Employee</div>
          <button @click="$emit('create-employee', false)">
            <i class="fas fa-times"></i>
        </button>
        </div>
        <div class="body">
          <div>
            <div class="label">ID</div>
            <input v-model="employeeCode" type="text" name="" placeholder="ID">
          </div>
          <div class="flex">
            <div>
              <div class="label">First Name</div>
              <input v-model="firstName" type="text" name="" placeholder="First Name">
            </div>
            <div>
              <div class="label">Last Name</div>
              <input v-model="lastName" type="text" name="" placeholder="Last Name">
            </div>
          </div>
          <div>
            <div class="label">Job Title</div>
            <input v-model="jobTitleName" type="text" name="" placeholder="Job Title">
          </div>
          <div>
            <div class="label">Email</div>
            <input v-model="emailAddress" type="email" name="" placeholder="Email">
          </div>
          <div>
            <div class="label">Phone Number</div>
            <input v-model="phoneNumber" type="text" name="" placeholder="Phone Number">
          </div>
          <div class="flex">
            <div>
              <div class="label">Region</div>
              <input v-model="region" type="text" name="" placeholder="Region">
            </div>
            <div>
              <div class="label">DOB</div>
              <input v-model="dob" type="date" name="" placeholder="DOB">
            </div>
          </div>
        </div>
        <div class="footer flex align-items justify-between">
          <button @click="addOrUpdateEmployee">{{ saveButtonTitle }}</button>
          <button @click="$emit('create-employee', false)"> Cancel </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CreateEmployee',
  props: {
    data: {
      type: Object,
      required: false
    }
  },
  data() {
    return {
      employeeCode: '',
      firstName: '',
      lastName: '',
      jobTitleName: '',
      emailAddress: '',
      phoneNumber: '',
      region: '',
      dob: ''
    };
  },
  created() {
    if(this.data) {
      // update form
      this.employeeCode = this.data.employeeCode;
      this.firstName = this.data.firstName;
      this.lastName = this.data.lastName;
      this.jobTitleName = this.data.jobTitleName;
      this.emailAddress = this.data.emailAddress;
      this.phoneNumber = this.data.phoneNumber;
      this.region = this.data.region;
      this.dob = this.data.dob.split("/").reverse().join("-");
    }
  },
  methods: {
    addOrUpdateEmployee() {
      if(this.data) {
        this.updateEmployee();
        return;
      }
      this.addAnEmployee();
    },
    addAnEmployee() {
      const postData = {
        employeeCode: this.employeeCode,
        firstName: this.firstName,
        lastName: this.lastName,
        preferredFullName: this.preferredFullName,
        jobTitleName: this.jobTitleName,
        emailAddress: this.emailAddress,
        phoneNumber: this.phoneNumber,
        region: this.region,
        dob: this.formattedDob
      };

      const url = `http://localhost:3000/employees`;
      const params = {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(postData)
      };
      
      fetch(url, params)
      .then((response) => {
        console.log("addAnEmployee -> response", response);
        this.$emit('create-employee', true);
      })
      .catch((err) => {
        console.log(err);
      });
    },
    getMonth(month) {
      const numericMonths = {
        0: '01',
        1: '02',
        2: '03',
        3: '04',
        4: '05',
        5: '06',
        6: '07',
        7: '08',
        8: '09',
        9: '10',
        10: '11',
        11: '12',
      };
      return numericMonths[month];
    },
    updateEmployee() {
      const postData = {
        employeeCode: this.employeeCode,
        firstName: this.firstName,
        lastName: this.lastName,
        preferredFullName: this.preferredFullName,
        jobTitleName: this.jobTitleName,
        emailAddress: this.emailAddress,
        phoneNumber: this.phoneNumber,
        region: this.region,
        dob: this.formattedDob
      };

      const url = `http://localhost:3000/employees/${this.data.id}`;
      const params = {
        method: 'PATCH',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(postData)
      };
      
      fetch(url, params)
      .then((response) => {
        console.log("addAnEmployee -> response", response);
        this.$emit('create-employee', true);
      })
      .catch((err) => {
        console.log(err);
      });
    }
  },
  computed: {
    preferredFullName() {
      return `${this.firstName} ${this.lastName}`;
    },
    formattedDob() {
      if (!this.dob) {
        return '';
      }

      const dateObj = new Date(this.dob);
      const date = dateObj.getDate() < 10 ? `0${dateObj.getDate()}` : dateObj.getDate();
      const month = dateObj.getMonth();
      const year = dateObj.getFullYear();
      const getMonth = this.getMonth(month) || '01';

      return `${date}/${getMonth}/${year}`;
    },
    saveButtonTitle() {
      return this.data ? 'Update' : 'Save';
    }
  }
}
</script>

<style>
.create-employee .modal .header, .create-employee .body, .create-employee .footer {
  padding: 1rem;
}
.create-employee .modal-content {
  width: 35%;
}
.create-employee .header {
  border-bottom: solid 1px darkgray;
}
.create-employee .footer {
  border-top: solid 1px darkgray;
}
.create-employee .modal .body > div {
  margin-bottom: 1rem;
}
.create-employee .modal .body .flex > * {
  flex: 1;
}
input {
  padding: 0.5rem;
  border: solid 1px black;
}
.label {
  margin-bottom: 0.5rem;
  color: lightslategray;
}
</style>