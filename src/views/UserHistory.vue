<template>
  <section class="user-history">
    <h1>Hi {{ username }}, you last changes!</h1>
    <hr>
    <table v-if="transactionsByUserId.length > 0">
      <thead>
        <th>Date</th>
        <th>Hour</th>
        <th>Id Origin</th>
        <th>Id Destiny</th>
        <th>Value</th>
        <th>Link</th>
      </thead>
      <tbody>
        <tr v-for="transaction in transactionsByUserId" :key="transaction.id">
          <td>{{ transaction.date.substring(0, 10) }}</td>
          <td>{{ transaction.date.substring(11, 19) }}</td>
          <td>{{ transaction.userIdOrigin }}</td>
          <td>{{ transaction.userIdDestiny }}</td>
          <td>${{ transaction.value }}</td>
          <td><router-link :to="'/transactions/' + transaction.id">Link</router-link></td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
import gql from 'graphql-tag'

export default {
  name: "UserHistory",

  data() {
    return {
      username: null,
      transactionsByUserId: []
    }
  },

  created() {
    this.username = localStorage.getItem('username')
  },

  apollo: {
    transactionsByUserId: {
      query: gql`
        query ($userId: String!) {
          transactionsByUserId(userId: $userId) {
            id
            userIdOrigin
            userIdDestiny
            value
            date
          }
        }
      `,

      variables: {
        userId: localStorage.getItem('userId')
      }
    }
  }
}
</script>

<style scoped>

</style>
