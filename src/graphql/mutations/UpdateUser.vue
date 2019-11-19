<template>
  <div>
    <ApolloMutation
      :mutation="require('../gqls/UpdateUser.gql')"
      :variables="{
        id: user.id,
        input: {
          username: user.username,
          email: user.email
        }
      }"
      :update="updateCache"
    >
      <template slot-scope="{ mutate }">
        <form @submit.prevent="updateUser(mutate)">
          <label>Username</label>
          <input type="text" v-model="user.username" />
          <label>Email</label>
          <input type="text" v-model="user.email" />
          <input type="submit" value="Update user" />
          <button class="button muted-button" @click="cancelEditing">Cancel</button>
        </form>
      </template>
    </ApolloMutation>
  </div>
</template>

<script>
export default {
  props: ["currentUser", "cancelEditing"],
  data: function() {
    return {
      user: {
        ...this.currentUser
      }
    };
  },
  methods: {
    updateUser(mutate) {
      if (!this.user.username || !this.user.email) return;

      mutate();

      this.cancelEditing();
    },
    updateCache(
      store,
      {
        data: { updateUser }
      }
    ) {
      const query = {
        query: require("../gqls/GetUsers.gql")
      };

      const data = store.readQuery(query);

      const index = data.getUsers.findIndex(u => u.id === updateUser.id);

      if (index !== -1) {
        data.getUsers[index] = updateUser;

        store.writeQuery({
          ...query,
          data
        });
      }
    }
  }
};
</script>

<style scoped></style>
