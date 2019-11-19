<template>
  <ApolloQuery :query="require('../gqls/GetUsers.gql')" :variables="{}">
    <template slot-scope="{ result: { loading, error, data } }">
      <!-- Loading -->
      <div v-if="loading" class="loading apollo">Loading...</div>

      <!-- Error -->
      <div v-else-if="error" class="error apollo">An error occured</div>

      <!-- Result -->
      <div v-else-if="data" class="result apollo">
        <UserTable :users="data.getUsers" :changeCurrentUser="changeCurrentUser" />
      </div>

      <!-- No result -->
      <div v-else class="no-result apollo">Loading...</div>
    </template>
  </ApolloQuery>
</template>

<script>
import UserTable from "../../components/UserTable.vue";

export default {
  props: ["changeCurrentUser"],
  components: {
    UserTable
  }
};
</script>

<style scoped>
.apollo,
.message {
  padding: 12px;
}

.error {
  color: red;
}
</style>
