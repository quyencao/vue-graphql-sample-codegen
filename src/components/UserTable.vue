<template>
  <table>
    <thead>
      <tr>
        <th>Username</th>
        <th>Email</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody v-if="users.length">
      <tr v-for="user in users" :key="user.id">
        <td>{{ user.username }}</td>
        <td>{{ user.email }}</td>
        <td>
          <button class="button muted-button" @click="changeCurrentUser(user)">Edit</button>
          <DeleteUser :id="user.id" />
        </td>
      </tr>
    </tbody>
    <tbody v-else>
      <tr>
        <td colspan="3">No users</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import DeleteUser from "../graphql/mutations/DeleteUser.vue";

export default {
  props: ["users", "changeCurrentUser"],
  components: {
    DeleteUser
  }
};
</script>

<style scoped></style>
