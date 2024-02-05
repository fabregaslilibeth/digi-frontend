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
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>

                    <v-text-field
                      v-model="newTask.title"
                      id="newTodo"
                      name="newTodo"
                      label="Type your task"
                      @keyup.enter="addingTask()"
                      :hint="todoExists ? 'Task already exists!' : ''"
                      persistent-hint
                    />
                  </v-list-item-title>
                </v-list-item-content>
                    
                <v-btn fab ripple small color="red" @click="addingTask()">
                  ADD TODO
                </v-btn>

              </v-list-item>

              <v-list-item v-for="task in data.tasks">
                <template >

                 <task :task="task" :key="task.id"></task>
                  
                </template>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>

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
`;

const { data } = useAsyncQuery(getTasksQuery, { variables: { limit: 5 } });

const createTaskMutation = gql`
  mutation createTask($title: String!){
    createTask (title: $title) {
      id
      title
      state
    }
  }
`;

const todoExists = computed(() => {
  // Check if the new task title already exists in the current list of tasks
  return data.value.tasks.some(task => task.title === newTask.value.title);
});

const newTask = ref({
  title: '',
});

const addingTask = async () => {
  try {
    await createTask({ title: newTask.value.title });
    newTask.value.title = ''; // Clear the input field after adding task
  } catch (error) {
    console.error('Error adding task:', error);
  }
};

const { mutate: createTask } = useMutation(createTaskMutation, {
  refetchQueries: [{ query: getTasksQuery }],
});


</script>