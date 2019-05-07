<template>
  <div id="dashboard">
    <!-- <h3>Dashboard</h3> -->
    <ul class="collection with-header">
      <li class="collection-header">
        <h6>Employees</h6>
      </li>
      <!-- our list -->
      <li v-for="employee in employees" v-bind:key="employee.id" class="collection-item">
        <div class="chip">{{employee.dept}}</div>
        {{employee.employee_id}}: {{employee.name}}
        <!-- <span>{{employee.dept}}</span> -->
        <!-- <span>{{employee.position}}</span> -->
        <router-link class="secondary-content" v-bind:to="{name: 'view-employee', params: {employee_id: employee.employee_id}}">
          <i class="fa fa-eye"></i>
        </router-link>
      </li>
    </ul>
    <!-- floting button -->
    <div class="fixed-action-btn">
      <router-link to="/new" class="btn-floating btn-large red">
        <i class="fa fa-plus"></i>
      </router-link>
    </div>

  </div>
</template>

<script>
import db from './firebaseinit'
export default {
  name: "dashboard",
  data() {
    return {
      employees: []
    }
  },
  created () {
    // db.collection('employees').get().then(querySnapshot => {
    db.collection('employees').orderBy('employee_id').get().then(querySnapshot => {
      querySnapshot.forEach(doc => {
        //console.log(doc.data());
        const data = {
          'id': doc.id,
          'employee_id': doc.data().employee_id,
          'name': doc.data().name,
          'dept': doc.data().dept,
          'position': doc.data().position,
        }
        // push data onto our "empty employees array"
        this.employees.push(data)
      })
    })
  }
};
</script>

