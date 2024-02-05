<template>
  <v-row>
    <v-list-item-action>
      <v-checkbox v-model="is_completed" @click="updatingTask()"></v-checkbox>
    </v-list-item-action>

    <v-list-item-content>
      <v-list-item-title :class="{done: active}">{{ task.title }} {{  task.state }} </v-list-item-title>
    </v-list-item-content>

    <v-btn fab ripple small color="red" @click="deletingTask()">
      <v-icon class="white--text">mdi-close</v-icon>
    </v-btn>

  </v-row>
</template>

<script lang="ts" setup>

const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
});

const is_completed = ref(props.task.state === 'done');

watchEffect(() => {
  is_completed.value = props.task.state === 'done';
});

const variables = {
  id: props.task.id,
  state: props.task.state === 'done' ? 'todo' : 'done'
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
  } catch (error) {
    console.error('Error deleting task:', error);
  }
};
</script>
