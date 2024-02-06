<template>
   <v-list-item>
    <template v-slot:prepend>
      <v-icon v-if="is_completed">mdi mdi-checkbox-marked-circle</v-icon>
      <v-icon v-else @click="updatingTask()">mdi mdi-checkbox-blank-circle-outline</v-icon>
    </template>

    <v-list-item-title v-text="task.title" :class="{done: is_completed}"></v-list-item-title>

    <template v-slot:append>
      <v-icon @click="deletingTask()">mdi-delete</v-icon>
    </template>
    </v-list-item>
</template>

<script setup lang="ts">

const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
});

const emits = defineEmits(['taskDeleted']); // Define custom event

const is_completed = ref(props.task.state === 'done');

watchEffect(() => {
  is_completed.value = props.task.state === 'done';
});

const variables = {
  id: props.task.id,
  state: 'done'
}

const updateTaskMutation = gql`
  mutation updateTask($id: ID!, $state: String!){
    updateTask (id: $id, state: $state) {
      id
      title
      state
    }
  }
`

const { mutate: updateTask } = useMutation(updateTaskMutation , { variables })

const updatingTask = async () => {
  try {
    await updateTask();
    is_completed.value = true
  } catch (error) {
    console.error('Error updating task:', error);
  }
};

const deleteTaskMutation  = gql`
  mutation deleteTask($id: ID!){
    deleteTask (id: $id) {
      id
      title
      state
    }
  }
`

const { mutate: deleteTask } = useMutation(deleteTaskMutation , {variables: {id: props.task.id}})

const deletingTask = async () => {
  try {
    await deleteTask();
    emits('taskDeleted', props.task.id); 
  } catch (error) {
    console.error('Error deleting task:', error);
  }
};

</script>

<style lang="scss">
.done {
  text-decoration: line-through;
}
</style>