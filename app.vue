<template>
  <v-container>
    <v-row justify="center" class="ma-5">
      <v-col xs="12" sm="8">
        <v-card>
          <v-toolbar color="teal darken-4" dark>
            <v-toolbar-title class="headline">To Do List</v-toolbar-title>
          </v-toolbar>

          <v-list subheader two-line flat>
            <v-list-item-group>
              <v-list-item v-for="task in data.tasks">
                <template >

                 <task :task="task" :key="task.id"></task>
                  
                </template>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>

        <v-btn fab ripple small color="red" @click="createTask()">
          ADD TODO
        </v-btn>

      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts" setup>

const getTasksQuery = gql`
  query getTasks{
    tasks {
      id
      title
      state
    }
  }
`

const createTaskMutation  = gql`
  mutation createTask($title: String!){
    createTask (title: $title) {
      id
      title
      state
    }
  }
`

const variables = {
  title: 'laundry',
}

const { mutate: createTask } = useMutation(createTaskMutation , {variables})

const { data } = await useAsyncQuery(getTasksQuery, {variables: {limit: 5}})
</script>