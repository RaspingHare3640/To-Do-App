<template>
    <div :key="task.id" v-for="task in tasks">
        <Task
        @edit-reminder="bubbleData" 
        @toggle-reminder="$emit('toggle-reminder', task.id)"
        @delete-task="$emit('delete-task', task.id)" :task="task" 
        />
    </div>
</template>



<script>
    import Task from "./Task"

    export default {
        name: "Tasks",
        props: {
            tasks: Array
        },
        components: {
            Task
        }, 
        methods: {
            bubbleData(data, id, type) {
                this.$emit('edit-reminder', data, id, type);
            }
        },
        emits: ['delete-task', 'toggle-reminder', "edit-reminder"],
        
    }
</script>