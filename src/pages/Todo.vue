<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        v-model="newTask"
        placeholder="Add Task"
        dense
        bg-color="white"
        filled
        square
        class="col"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-ripple
        v-for="(task, index) in tasks"
        :key="task.id"
        @click="task.done = !task.done"
        clickable
        :class="{ done: task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            val="teal"
            color="teal"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            color="primary"
            dense
            icon="delete"
            @click.stop="deleteTask(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center" v-if="!tasks.length">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "Todo",
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   id: 1,
        //   title: "Cuci Piring",
        //   done: false,
        // },
        // {
        //   id: 2,
        //   title: "Cuci Mobil",
        //   done: true,
        // },
        // {
        //   id: 3,
        //   title: "Masak",
        //   done: true,
        // },
        // {
        //   id: 4,
        //   title: "Mandi",
        //   done: false,
        // },
      ],
      check: "",
    };
  },
  methods: {
    addTask() {
      this.tasks.push({
        id: this.tasks.length + 1,
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Delete Task",
          message: "Are you sure to delete this task?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("task deleted");
        });
    },
  },
};
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>