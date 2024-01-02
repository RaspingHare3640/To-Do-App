<template>
    <footer>
        <p>Copyright &copy; 2024</p>
        <div class="links">
            <router-link @click="handleHomePage" v-show="!onHomePage" class="link" to="/">Go Back</router-link>
            <router-link @click="handleHomePage" v-show="onHomePage" class="link" to="/about">About</router-link>
        </div>
        
    </footer>
</template>
<script>
    export default {
        name: "Footer",
        props: {
            onHomePage: Boolean
        },
        methods: {
            handleHomePage() {
                this.$emit("handleHomePage");
            }
        },
        emits: ["handleHomePage"]
    }
</script>
<style scoped>
    a {
        color: #333;

    }

    footer {
        margin-top: 30px;
        text-align: center;
    }

    .links {
        margin:1em;
    }
    .link {
        text-decoration: none;
        border: 2px solid transparent;
        color: black;
        transition: border 0.5s ease-in-out;
        transition: color 0.5 ease-in-out;
        margin: 0.5em;
        padding: 0.5em;
    }

    .link:hover {
        border: 2px solid rgb(22, 238, 22);
        color: blue;
    }
</style>