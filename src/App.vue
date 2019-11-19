<template>
  <div class="container">
    <h1>Vue Graphql Apollo Sample</h1>
    <div class="flex-row">
      <div class="flex-large">
        <template v-if="isEditing">
          <h2>Edit user</h2>
          <UpdateUser :currentUser="currentUser" :cancelEditing="cancelEditing" />
        </template>
        <template v-else>
          <h2>Add User</h2>
          <CreateUser />
        </template>
      </div>
      <div class="flex-large">
        <h2>View users</h2>
        <GetUsers :changeCurrentUser="changeCurrentUser" />
      </div>
    </div>
  </div>
</template>

<script>
import GetUsers from "./graphql/queries/GetUsers.vue";
import CreateUser from "./graphql/mutations/CreateUser.vue";
import UpdateUser from "./graphql/mutations/UpdateUser.vue";

export default {
  name: "app",
  data: function() {
    return {
      currentUser: {
        id: null,
        username: "",
        email: ""
      },
      isEditing: false
    };
  },
  components: {
    GetUsers,
    CreateUser,
    UpdateUser
  },
  methods: {
    changeCurrentUser(user) {
      this.isEditing = true;
      this.currentUser = user;
    },
    cancelEditing() {
      this.isEditing = false;
    }
  }
};
</script>

<style></style>
