<template>
  <div>
  {{  data.tasks }}
  <p>There are {{ data?.tasks?.length || 0 }} tasks.</p>
    <v-btn>Add Task</v-btn>
  </div>
</template>

<script lang="ts" setup>
const query = gql`
  query getTasks{
    tasks {
      id
      title
    }
  }
`

const variables = { limit: 5 }

const { data } = await useAsyncQuery(query, variables)
console.log(data._value)
</script>