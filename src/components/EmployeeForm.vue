<template>
  <div class="main-wrapper">
    <div class="container py-4">
      <!-- HEADER -->
      <div class="text-center mb-5">
        <h1 class="fw-bold text-primary display-5">
          Employee Management System
        </h1>
        <p class="text-muted fs-5">
          CRUD Operations using Vue.js, Axios & MockAPI
        </p>
      </div>

      <!-- CREATE -->
      <div class="card shadow-lg border-0 rounded-4 mb-5">
        <div class="card-header create-header text-white">
          <h3>Create Employee (POST)</h3>
        </div>

        <div class="card-body p-4">
          <form @submit.prevent="saveEmployee">
            <div class="row g-3">
              <div class="col-lg-4 col-md-6 col-12">
                <input v-model="employee.employeeId" type="number"
                  class="form-control form-control-lg"
                  placeholder="Employee ID" required>
              </div>

              <div class="col-lg-4 col-md-6 col-12">
                <input v-model="employee.name" type="text"
                  class="form-control form-control-lg"
                  placeholder="Employee Name" required>
              </div>

              <div class="col-lg-4 col-md-6 col-12">
                <input v-model="employee.designation" type="text"
                  class="form-control form-control-lg"
                  placeholder="Designation" required>
              </div>

              <div class="col-lg-6 col-md-6 col-12">
                <input v-model="employee.department" type="text"
                  class="form-control form-control-lg"
                  placeholder="Department" required>
              </div>

              <div class="col-lg-6 col-md-6 col-12">
                <input v-model="employee.salary" type="number"
                  class="form-control form-control-lg"
                  placeholder="Salary" required>
              </div>
            </div>

            <div class="text-center mt-4">
              <button class="btn btn-success btn-lg px-5">
                {{ isEditing ? "Update Employee" : "Add Employee" }}
              </button>
            </div>
          </form>
        </div>
      </div>

      <!-- READ -->
      <div class="card shadow-lg border-0 rounded-4 mb-5">
        <div class="card-header read-header text-white">
          <h3>Read Employees (GET)</h3>
        </div>

        <div class="card-body table-responsive p-4">
          <table class="table table-hover text-center align-middle">
            <thead>
              <tr>
                <th>Employee ID</th>
                <th>Name</th>
                <th>Designation</th>
                <th>Department</th>
                <th>Salary</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="emp in employees" :key="emp.id">
                <td>{{ emp.employeeId }}</td>
                <td>{{ emp.name }}</td>
                <td>{{ emp.designation }}</td>
                <td>{{ emp.department }}</td>
                <td>₹ {{ emp.salary }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- DELETE -->
      <div class="card shadow-lg border-0 rounded-4 mb-5">
        <div class="card-header delete-header text-white">
          <h3>Delete Employee (DELETE)</h3>
        </div>

        <div class="card-body table-responsive p-4">
          <table class="table table-hover text-center align-middle">
            <thead>
              <tr>
                <th>Name</th>
                <th>Designation</th>
                <th>Salary</th>
                <th>Action</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="emp in employees" :key="emp.id">
                <td>{{ emp.name }}</td>
                <td>{{ emp.designation }}</td>
                <td>₹ {{ emp.salary }}</td>
                <td>
                  <button
                    class="btn btn-danger btn-sm px-3"
                    @click="deleteEmployee(emp.id)"
                  >
                    Delete
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- UPDATE -->
      <div class="card shadow-lg border-0 rounded-4 mb-5">
        <div class="card-header update-header text-dark">
          <h3>Update Employee (PUT)</h3>
        </div>

        <div class="card-body table-responsive p-4">
          <table class="table table-hover text-center align-middle">
            <thead>
              <tr>
                <th>Name</th>
                <th>Designation</th>
                <th>Salary</th>
                <th>Action</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="emp in employees" :key="emp.id">
                <td>{{ emp.name }}</td>
                <td>{{ emp.designation }}</td>
                <td>₹ {{ emp.salary }}</td>
                <td>
                  <button
                    class="btn btn-warning btn-sm px-3"
                    @click="editEmployee(emp)"
                  >
                    Edit
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const API_URL = "https://69f5b284fb098eb7f0b5777a.mockapi.io/api/employee";

export default {
  data() {
    return {
      employees: [],
      employee: {
        employeeId: "",
        name: "",
        designation: "",
        department: "",
        salary: ""
      },
      isEditing: false,
      editId: null
    };
  },

  mounted() {
    this.fetchEmployees();
  },

  methods: {
    async fetchEmployees() {
      const response = await axios.get(API_URL);
      this.employees = response.data;
    },

    async saveEmployee() {
      if (this.isEditing) {
        await axios.put(`${API_URL}/${this.editId}`, this.employee);
        alert("Employee Updated Successfully");
        this.isEditing = false;
        this.editId = null;
      } else {
        await axios.post(API_URL, this.employee);
        alert("Employee Added Successfully");
      }

      this.resetForm();
      await this.fetchEmployees();
    },

    editEmployee(emp) {
      this.employee = { ...emp };
      this.editId = emp.id;
      this.isEditing = true;

      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    },

    async deleteEmployee(id) {
      await axios.delete(`${API_URL}/${id}`);
      alert("Employee Deleted Successfully");
      await this.fetchEmployees();
    },

    resetForm() {
      this.employee = {
        employeeId: "",
        name: "",
        designation: "",
        department: "",
        salary: ""
      };
    }
  }
};
</script>

<style>
body {
  background: #f4f7fb;
  font-family: Arial, sans-serif;
}

.create-header {
  background: linear-gradient(135deg, #11998e, #38ef7d);
}

.read-header {
  background: linear-gradient(135deg, #396afc, #2948ff);
}

.delete-header {
  background: linear-gradient(135deg, #ff416c, #ff4b2b);
}

.update-header {
  background: linear-gradient(135deg, #f7971e, #ffd200);
}

table th {
  background-color: #212529;
  color: white;
}

.card {
  overflow: hidden;
}

@media (max-width: 768px) {
  h1 {
    font-size: 2rem;
  }

  .card-header h3 {
    font-size: 1.2rem;
  }

  table {
    font-size: 14px;
  }
}
</style>