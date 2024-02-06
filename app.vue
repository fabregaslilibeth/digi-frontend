<template>
  <v-card
   class="mx-auto"
   max-width="700"
  >
   <v-list density="compact">
     <v-list-subheader>TO DO LIST</v-list-subheader>
     <v-list-item>
       <v-text-field
         v-model="newTask.title"
         id="newTodo"
         name="newTodo"
         label="Type your task, hit enter to add"
         @keyup.enter="addingTask()"
         :hint="todoExists ? 'Task already exists!' : ''"
         persistent-hint
       />
     </v-list-item>
   </v-list>

   <v-list density="compact">
     <task v-for="(task, i) in data?.tasks" :task="task" :key="task.id" @taskDeleted="removeTask"></task>
   </v-list>
 </v-card>
</template>

<script lang="ts" setup>
const getTasksQuery = gql`
 query getTasks {
   tasks {
     id
     title
     state
   }
 }
`;

const createTaskMutation = gql`
 mutation createTask($title: String!) {
   createTask(title: $title) {
     id
     title
     state
   }
 }
`;

const { data, refresh } = useAsyncQuery(getTasksQuery);

const newTask = ref({
 title: '',
});

const { mutate: createTask } = useMutation(createTaskMutation, {
 refetchQueries: [{ query: getTasksQuery }],
});

const todoExists = computed(() => {
 return data.value?.tasks.some(task => task.title === newTask.value.title);
});

const addingTask = async () => {
 try {
    if (todoExists.value) {
      // Task already exists, do not create a new one
      return;
    }

   const response = await createTask({ title: newTask.value.title });
   const createdTask = response.data.createTask;
   data.value.tasks.push(createdTask); // Update local state with the new task
   newTask.value.title = ''; // Clear the input field after adding task
 } catch (error) {
   console.error('Error adding task:', error);
 }
};

const removeTask = (taskId) => {
  const index = data.value.tasks.findIndex(task => task.id === taskId);
  if (index < 0) {
      // Task is not found
      return;
    }
  data.value.tasks.splice(index, 1)
}
</script>
