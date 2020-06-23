<template>
  <div class="view-employee">
    <div class="modal">
      <div class="modal-content">
        <div class="header flex align-items justify-between">
          <div class="title">{{ data.preferredFullName }}</div>
          <button @click="$emit('view-employee', false)">
            <i class="fas fa-times"></i>
        </button>
        </div>
        <div class="body">
          <table>
            <tr>
              <td>Name</td>
              <td>{{ data.preferredFullName }}</td>
            </tr>
            <tr>
              <td>Employee Code</td>
              <td>{{ data.employeeCode }}</td>
            </tr>
            <tr>
              <td>Job Title</td>
              <td>{{ data.jobTitleName }}</td>
            </tr>
            <tr>
              <td>Phone Number</td>
              <td>{{ data.phoneNumber }}</td>
            </tr>
            <tr>
              <td>Email ID</td>
              <td>{{ data.emailAddress }}</td>
            </tr>
            <tr>
              <td>Region</td>
              <td>{{ data.region }}</td>
            </tr>
            <tr>
              <td>DOB</td>
              <td>{{ data.dob }}</td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ViewEmployee',
  props: {
    data: {
      type: Object,
      required: true
    }
  }
}
</script>

<style>
.view-employee .modal-content {
  width: 35%;
}
.view-employee .header, .view-employee .body, .view-employee .footer {
  padding: 2rem;
}
.view-employee .header {
  border-bottom: solid 1px darkgray;
}
.view-employee table td {
  border: none;
}
</style>