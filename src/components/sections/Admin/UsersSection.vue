<template>
  <v-main>
    <v-container class="mt-5 section2">
      <v-row justify="center">
        <v-col cols="12">
          <h2 class="text-center">USER DETAILS</h2>
        </v-col>
      </v-row>

      <v-row justify="end">
        <v-col cols="12" sm="3">
          <v-btn color="success" block @click="showUserForm">Add User</v-btn>
        </v-col>
      </v-row>

      <v-row justify="center">
        <v-col cols="12">
          <CustomTable :columns="tableColumns" :items="users" :showEditIcon="true" :showDeleteIcon="true"
            @edit-data="editUserRow" @delete-data="deleteUserRow" height="445px" />
        </v-col>
      </v-row>
      <v-row justify="center" v-if="showForm">
        <v-col cols="12" sm="6">
          <UserForm @add="addUser" @update="updateUser(editingUserIndex, $event)" @cancel="hideUserForm"
            :initialUser="editingUser" />
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</template>
  
<script>
import CustomTable from '../../common/CustomTable.vue';
import UserForm from '../../common/UserForm.vue';

export default {
  name: 'UsersDetails',
  components: {
    CustomTable,
    UserForm,
  },

  data() {
    return {
      showForm: false,
      users: [],
      editingUser: null,
      editingUserIndex: -1,
      tableColumns: [
        { key: 'userType', label: 'User Type' },
        { key: 'userName', label: 'User Name' },
        { key: 'firstName', label: 'First Name' },
        { key: 'lastName', label: 'Last Name' },
        { key: 'gender', label: 'Gender' },
        { key: 'age', label: 'Age' },
        { key: 'address', label: 'Address' },
      ],

    };
  },
  
  methods: {
    showUserForm() {
      this.showForm = true;
    },

    hideUserForm() {
      this.showForm = false;
      this.editingUser = null;
      this.editingUserIndex = -1;
    },

    addUser(userData) {
      if (!userData.userType || !userData.userName || !userData.firstName ||
        !userData.lastName || !userData.gender || !userData.age || !userData.address) {
        return;
      }
      this.users.push(userData);
      this.hideUserForm();
    },

    updateUser(index, updatedUser) {
      this.users[index] = updatedUser;
      this.editingUser = null;
      this.hideUserForm();
    },

    editUserRow(user) {
      this.editingUser = { ...user };
      const index = this.users.findIndex(p => p.userCode === user.userCode);
      this.editingUserIndex = index;
      this.showForm = true;
    },

    deleteUserRow(user) {
      const index = this.users.findIndex(p => p.userCode === user.userCode);
      if (index !== -1) {
        this.users.splice(index, 1);
      }
    },

  },
};
</script>
  
<style scoped>

</style>
  