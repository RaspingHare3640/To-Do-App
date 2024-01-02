<template>
    <div  
    :class="[task.reminder ? 'reminder' : '', 'task' ]"
    > 
        <form @submit.prevent="bubbleData">
          <input class="edit" @keydown.esc="handleEscape" placeholder="Enter New Reminder" v-model="textData" v-show="isEditText" type="text">
          <input class="edit" @keydown.esc="handleEscape" placeholder="Enter New Date" v-model="dateData" v-show="isEditDate" type="text">
          <button type="submit" style="display: none;"></button>
        </form>
        <input type="checkbox" v-model="task.reminder" @change="$emit('toggle-reminder', task.id)" >

         <h3>
            <div @click="editTask">
              {{task.text}}
            </div>
            <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
         </h3>

         <p @click="editDate">{{task.day}}</p>
    </div>
    
   
</template>

<script>
    export default {
        name: "Task",
        props: {
            task: Object
        },
        data () {
          return {
            isEditText: false,
            isEditDate: false,
            textData: "",
            dateData: "",
            type: "",
          }
        },

        methods: {
          editTask() {
            if (!this.isEditDate) {
                this.isEditText = !this.isEditText;
                this.textData = ""
            }
            else {
              alert("Please Finish Entering This Field First");
            }
          },
          editDate() {
            if (!this.isEditText) {
                this.isEditDate = !this.isEditDate;
                this.dateData = ""
            }
            else {
              alert("Please Finish Entering This Field First");
            }
          },
          bubbleData() {

            
            
            if (this.isEditText) {
              if (this.textData.length == 0) {
                alert("Please Enter a Value");
                return
              }
              this.type = "text";
              this.$emit("edit-reminder", this.textData, this.task.id, this.type);
              this.isEditText = false;
              console.log('run')
              
            }
            else if (this.isEditDate) {
              if (this.dateData.length == 0) {
                alert("Please Enter a Value");
                return
              }
              this.type = "date";
              this.$emit("edit-reminder", this.dateData, this.task.id, this.type);
              this.isEditDate = false;
            }

            this.dateData = ""
            this.textData = ""
            
          },
          handleEscape(event) {
            if (event.keyCode === 27) {
              this.isEditText = false;
              this.isEditDate = false;
              this.dateData = ""
              this.textData = ""
              console.log("run")
            }
          },

          rm () {
            console.log(this.reminder);
          }
        },
        emits: ["delete-task", "toggle-reminder", "edit-reminder"]
    }
  
</script>

<style>
    .fas {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.edit {
  border: none;
  outline: none;
  background-color: transparent;
  border-bottom: 1px solid transparent;
  transition: border-bottom 0.5s ease-in-out;
  padding: 0.5em;
}

.edit:focus {
  border-bottom: 2px solid green;
}

.edit:hover {
  border-bottom: 2px solid green;
}
</style>