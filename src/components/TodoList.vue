<template>
  <v-container fluid>
    <v-row>
      <v-col cols="12" md="6" class="mx-auto">
        <v-card class="pa-4" elevation="2">
          <v-text-field
            v-model="newTask"
            label="Add a new task"
            outlined
            dense
            class="task-input"
            @keyup.enter="addTask"
          ></v-text-field>
          <v-btn @click="addTask" class="mt-2 bg-purple-darken-3">
            Add Task
          </v-btn>

          <div class="task-list-scroll mt-4">
            <v-list>
              <v-list-item-group v-if="tasks.length">
                <v-list-item 
                  v-for="(task, index) in tasks" 
                  :key="index" 
                  class="task-list-item"
                >
                  <v-list-item-content>
                    <v-text-field
                      v-if="editIndex === index"
                      v-model="editedTask"
                      outlined
                      dense
                      class="task-input"
                      @keyup.enter="saveTask(index)"
                      @keyup.esc="cancelEdit"
                    ></v-text-field>
                    <v-list-item-title v-else>
                      {{ task }}
                    </v-list-item-title>
                  </v-list-item-content>
                  <v-list-item-action>
                    <v-btn @click="editTask(index)" class="mx-1 bg-pink-lighten-3" v-if="editIndex !== index">
                      Edit
                    </v-btn>
                    <v-btn @click="saveTask(index)" class="mx-1 bg-pink-accent-4" v-if="editIndex === index">
                      Save
                    </v-btn>
                    <v-btn @click="removeTask(index)" class="mx-1 bg-pink-darken-4">
                      Delete
                    </v-btn>
                  </v-list-item-action>
                </v-list-item>
              </v-list-item-group>
              <v-list-item v-else>
                <v-list-item-content class="text-center">
                  No tasks available
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      editIndex: null,
      editedTask: ''
    };
  },
  created() {
    this.loadTasks();
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push(this.newTask.trim());
        this.newTask = '';
        this.saveTasks();
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      if (this.editIndex === index) {
        this.cancelEdit();
      }
      this.saveTasks();
    },
    editTask(index) {
      this.editIndex = index;
      this.editedTask = this.tasks[index];
    },
    saveTask(index) {
      if (this.editedTask.trim()) {
        this.tasks[index] = this.editedTask.trim(); // Directly update the task
        this.cancelEdit();
        this.saveTasks();
      }
    },
    cancelEdit() {
      this.editIndex = null;
      this.editedTask = '';
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    loadTasks() {
      const savedTasks = localStorage.getItem('tasks');
      if (savedTasks) {
        this.tasks = JSON.parse(savedTasks);
      }
    }
  }
};
</script>

<style scoped>
.task-list-scroll {
  height: 500px; /* Adjust this height as per your needs */
  overflow-y: auto; /* Enable vertical scrolling */
}

.task-list-scroll::-webkit-scrollbar {
  width: 8px; /* Width of the scrollbar */
}

.task-list-scroll::-webkit-scrollbar-track {
  background: #f1f1f1; /* Background color of the scrollbar track */
  border-radius: 4px; /* Rounded corners for the track */
}

.task-list-scroll::-webkit-scrollbar-thumb {
  background: #888; /* Color of the scrollbar thumb */
  border-radius: 4px; /* Rounded corners for the thumb */
}

.task-list-scroll::-webkit-scrollbar-thumb:hover {
  background: #555; /* Color of the scrollbar thumb on hover */
}

.task-input {
  max-width: 100%; /* Adjusted for better responsiveness */
  margin-right: 16px;
}

.v-list-item {
  border-bottom: 1px solid #e0e0e0;
  transition: background-color 0.3s, box-shadow 0.3s;
}

.v-list-item:hover {
  background-color: #f5f5f5; /* Light grey background on hover */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Subtle shadow on hover */
}

.v-list-item-title {
  font-size: 1rem;
}

.v-btn {
  margin-left: 8px;
  text-transform: none; /* Keeps the text in its normal case */
  border-radius: 4px; /* Makes the button rectangular */
  transition: background-color 0.3s, box-shadow 0.3s; /* Smooth transition for hover effect */
}

.v-btn:hover {
  background-color: rgba(0, 0, 0, 0.1); /* Light background on hover */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Slight shadow on hover */
}

.v-list-item-action {
  display: flex;
  align-items: center;
}

.v-list-item-content {
  text-align: left; /* Align text to the left */
}

.v-card {
  padding: 16px;
}
</style>
