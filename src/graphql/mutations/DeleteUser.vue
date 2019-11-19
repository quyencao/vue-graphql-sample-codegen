<template>
  <ApolloMutation
    :mutation="require('../gqls/DeleteUser.gql')"
    :variables="{
    id: id
  }"
    :update="updateCache"
    class="inline"
  >
    <template slot-scope="{ mutate }">
      <button class="button muted-button" @click="mutate()">Delete</button>
    </template>
  </ApolloMutation>
</template>

<script>
export default {
  props: ["id"],
  methods: {
    updateCache(store) {
      const query = {
        query: require("../gqls/GetUsers.gql")
      };

      const data = store.readQuery(query);

      const index = data.getUsers.findIndex(u => u.id === this.id);

      if (index !== -1) {
        data.getUsers.splice(index, 1);

        store.writeQuery({
          ...query,
          data
        });
      }
    }
  }
};
</script>

<style scoped>
.inline {
  display: inline;
}
</style>
