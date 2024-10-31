<template>
  <div class="app form-background">
    <h1 class="title">User Management</h1>
    

    <label for="info" class="label">Enter User Info</label>

    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1"><i class="bi bi-person"></i></span>
      <input type="text" class="form-control background-input" v-model="username" placeholder="Username" aria-label="Username" aria-describedby="basic-addon1">
    </div>

    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1"><i class="bi bi-envelope-open-fill"></i></span>
      <input type="text" class="form-control background-input" v-model="email" placeholder="Email" aria-label="Email" aria-describedby="basic-addon1">
    </div>

    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1"><i class="bi bi-lock-fill"></i></span>
      <input type="password" class="form-control background-input" v-model="password" placeholder="Password" aria-label="Password" aria-describedby="basic-addon1">
    </div>

    <button type="button" class="btn btn-primary" @click="addUserInfo">Create</button>
    
    <h4 class="mt-5">User Info Table</h4>
    <table class="table table-bordered" id="userInfoTable">
      <thead>
        <tr>
          <th style="color: white; background-color: #191970;">Username</th>
          <th style="color: white; background-color: #191970;">Email</th>
          <th style="color: white; background-color: #191970">Password</th>
          <th style="color: white; background-color: #191970;">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in users" :key="index">
          <td v-if="user.isEditing">
            <input type="text" v-model="user.username" class="form-control">
          </td>
          <td v-else>{{ user.username }}</td>

          <td v-if="user.isEditing">
            <input type="text" v-model="user.email" class="form-control">
          </td>
          <td v-else>{{ user.email }}</td>

          <td v-if="user.isEditing">
            <input type="password" v-model="user.password" class="form-control">
          </td>
          <td v-else>{{ user.password }}</td>

          <td>
            <button class="btn btn-warning btn-sm" @click.stop="toggleEditMode(index)">
              <i class="bi" :class="user.isEditing ? 'bi-check-lg' : 'bi-pencil-square'"></i>
            </button>
            <button class="btn btn-danger btn-sm" @click.stop="deleteUserInfo(index)">
              <i class="bi bi-trash-fill"></i>
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <div v-if="message" class="alert alert-success message-alert mt-3">{{ message }}</div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const username = ref('');
    const email = ref('');
    const password = ref('');
    const users = ref([]);
    const message = ref('');

    const isDuplicate = () => {
      return users.value.some(
        user => user.username === username.value || user.email === email.value
      );
    };

    const addUserInfo = () => {
      if (!username.value || !email.value || !password.value) {
        alert('All fields are required!');
        return;
      }

      if (isDuplicate()) {
        alert('User with the same username or email already exists!');
        return;
      }

      users.value.push({
        username: username.value,
        email: email.value,
        password: password.value,
        isEditing: false // Track edit mode per user
      });
      message.value = 'User Created Successfully';
      resetForm();
    };

    const toggleEditMode = (index) => {
      users.value[index].isEditing = !users.value[index].isEditing;
      
      if (!users.value[index].isEditing) {
        // If exiting edit mode, show "User Updated" message
        message.value = 'User Updated Successfully';
        setTimeout(() => {
          message.value = '';
        }, 2000);
      }
    };

    const deleteUserInfo = (index) => {
      users.value.splice(index, 1);
      message.value = 'User Deleted Successfully';
      setTimeout(() => {
        message.value = '';
      }, 2000);
    };

    const resetForm = () => {
      username.value = '';
      email.value = '';
      password.value = '';
      setTimeout(() => {
        message.value = '';
      }, 2000);
    };

    return {
      username,
      email,
      password,
      users,
      message,
      addUserInfo,
      toggleEditMode,
      deleteUserInfo
    };
  }
};
</script>

<style scoped>
.form-background {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    height: 100vh;
   
}

.title {
    margin-top: 0px;
    margin-left: 475px;
    text-align: center;
    width: 350%;
    background-color: #191970;
    font-family: "Times New Roman", Times, serif;
    color: white;
    padding: 5px;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.input-group {
    margin-top: 0px;
    margin-left: 400px;
    text-align: center;
    width: 50%;  
}

.btn-primary {
    margin-top: 0px;
    margin-left: 390px;
    text-align: center;
    width: 10%;  
}

.mt-5 {
    margin-bottom: 10px;
    margin-left: 400px;
    font-family: "Times New Roman", Times, serif;
    text-align: center;
    width: 100%; 
}

.label {
    margin-top: 10px;
    margin-left: 400px;
    font-family: "Times New Roman", Times, serif;
    text-align: center;
    width: 20%;  
}

.table {
    margin-top: 10px;
    margin-left: 400px;
    width: 150%;
    border: 1px solid black; /* Set border color to black */
}

.table-bordered {
    border-collapse: collapse; /* Ensure table borders are collapsed */
}

.table-bordered th,
.table-bordered td {
    border: 1px solid black; /* Set individual cell border color to black */
}

.bordered-input {
    border: 2px solid #007bff; /* Blue border */
    border-radius: 0.25rem;    /* Slightly rounded corners */
}

.bordered-input:focus {
    border-color: #0056b3; /* Darker blue on focus */
    box-shadow: 0 0 0.2rem rgba(0, 123, 255, 0.25); /* Optional shadow */
}

.background-input {
    background-color: #f8f9fa; /* Light gray background for inputs */
    border: 1px solid #ced4da; /* Border color */
}

.message-alert {
    margin-left: 400px;
    text-align: center;
    width: 35%;
}
</style>
