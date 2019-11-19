<template>
  <div>
    <ApolloMutation
      :mutation="require('../gqls/CreateUser.gql')"
      :variables="{
        input: {
          username: username,
          email: email
        }
      }"
      :update="updateCache"
    >
      <template slot-scope="{ mutate }">
        <form @submit.prevent="addNewUser(mutate)">
          <label>Username</label>
          <input type="text" v-model="username" />
          <label>Email</label>
          <input type="text" v-model="email" />
          <input type="submit" value="Add new user" />
        </form>
      </template>
    </ApolloMutation>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      username: "",
      email: ""
    };
  },
  methods: {
    addNewUser(mutate) {
      if (!this.username || !this.email) return;

      mutate();

      this.username = "";
      this.email = "";
    },
    updateCache(
      store,
      {
        data: { createUser }
      }
    ) {
      const query = {
        query: require("../gqls/GetUsers.gql")
      };

      const data = store.readQuery(query);

      data.getUsers.push(createUser);

      store.writeQuery({
        ...query,
        data
      });
    }
  }
};
</script>

<style scoped></style>
