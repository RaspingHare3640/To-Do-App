<template>
   <AddTask v-show="showAddTask" @add-task="addTask"/>
   <Tasks @edit-reminder="editReminder" @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
    <!-- <Time /> -->
</template>

<script>
import AddTask from "../components/AddTask"
import Time from "../components/Time"
import Tasks from "../components/Tasks"

export default {
    name: "Home",
    props: {
        showAddTask: Boolean,
    },
    components: {
        Tasks,
        AddTask,
        Time,
    }, 
    data() {
        return {
            tasks: []
        }
    },
    methods: {
    // Adds the task to the tasks array
    // Fetches updates the backend as well
    async addTask(task) {
      // Backemd POST request
      const res = await fetch('api/tasks', {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task)
      })

      const data = await res.json();
      this.tasks = [...this.tasks, data]
    },

    // Deletes the task
    // Updates the backend as well
    async deleteTask(id) {
      if(confirm("Are you sure?")) {
        // Makes a delete Request to the Back end
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        })
        
        // makes sure the request went through
        res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert("Error Deleting Task")
      }
    },

    async editReminder (fdata, id, type) {
      const taskToEdit = await this.fetchTask(id);
      // Handle Text updates
      if (type == "text") {
        // Makes the request
        const updTask = {...taskToEdit, text: fdata};
        const res = await fetch(`api/tasks/${id}`, {
          method: "PUT",
          headers: {
            "Content-type": "application/json",
          },
          body : JSON.stringify(updTask)
        })

        // Recieves the data
        const data = await res.json();
        // Updates the UI from the data
        this.tasks = this.tasks.map((task) => 
          task.id == id ? {...task, text: data.
          text} : task
        )
      
      } 
      // Handles Date changes
      else {
        // makes the Request
        const updTask = {...taskToEdit, day: fdata};
        const res = await fetch(`api/tasks/${id}`, {
          method: "PUT",
          headers: {
            "Content-type": "application/json",
          },
          body : JSON.stringify(updTask)
        })

        // Recives the data back from  backend
        const data = await res.json();
        
        // Updates UI with backend data
        this.tasks = this.tasks.map((task) => 
          task.id == id ? {...task, day: data.
          day} : task
        )
      }

      console.log(this.tasks)
    },

    // Toggles the reminder
    // updates the backend
    async toggleReminder(id) {
      // gets the task we want to change
      const taskToToggle = await this.fetchTask(id);
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder};

      // backend request
      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body : JSON.stringify(updTask),
      });

      // Recieves the data
      const data = await res.json();

      // Updates the task in UI
      this.tasks = this.tasks.map((task) => 
        task.id == id ? {...task, reminder: data.
        reminder} : task
      )
    },

    // Gets Taks saved on Json Server (db.json file)
    async fetchTasks() {
      const res = await fetch("api/tasks")

      const data = await res.json()

      return data

    },
    // Gets an induvidual task given an id
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`)

      const data = await res.json()

      return data

    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>

