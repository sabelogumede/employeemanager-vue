<template>
  <div id="view-employee">
    <!-- <h3>View Employee</h3> -->
    <ul class="collection with-header">
      <li class="collection-header">
        <h4>{{name}}</h4>
      </li>
      <li class="collection-item">Employee ID#: {{employee_id}}</li>
      <li class="collection-item">Department: {{dept}}</li>
      <li class="collection-item">Position: {{position}}</li>
    </ul>
    <!-- back-home-button -->
    <router-link to="/" class="btn grey">Back</router-link>
    <!-- Delete-button -->
    <button @click="deleteEmployee" class="btn red">Delete</button>

    <!-- floting button -->
    <div class="fixed-action-btn">
      <router-link v-bind:to="{name: 'edit-employee', params: {employee_id: employee_id}}"
        class="btn-floating btn-large red">
        <i class="fas fa-edit"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
// import db
import db from "./firebaseinit";
export default {
  name: "view-employee",
  data() {
    return {
      employee_id: null,
      name: null,
      dept: null,
      position: null
    };
  },
  //lifecyle Method - runs before the page loads
  beforeRouteEnter(to, from, next) {
    db.collection("employees")
      .where("employee_id", "==", to.params.employee_id)
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.employee_id = doc.data().employee_id;
            vm.name = doc.data().name;
            vm.dept = doc.data().dept;
            vm.position = doc.data().position;
          });
        });
      });
  },
  // watcher
  watch: {
    $route: "fetchData"
  },
  methods: {
    fetchData() {
      //get employee with equal/== id as in the router params
      db.collection("employees")
        .where("employee_id", "==", this.$route.params.employee_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            // set our property value
            this.employee_id = doc.data().employee_id;
            this.name = doc.data().name;
            this.dept = doc.data().dept;
            this.position = doc.data().position;
          });
        });
    },
    deleteEmployee() {
      if (confirm("Are you sure?")) {
        // --------
        db.collection("employees")
          .where("employee_id", "==", this.$route.params.employee_id)
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              // delete using ref
              doc.ref.delete();
              //then redirect us
              this.$router.push("/");
            });
          });
        // --------
      }
    }
    // ---delete
  }
};
</script>