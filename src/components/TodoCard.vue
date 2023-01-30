<template>
    <div>
        <span> {{ title }} </span>
        <button class="btn btn-success" v-if="state" @click="$emit('change-state')"
        style="--bs-btn-padding-y: .25rem; --bs-btn-padding-x: .5rem; --bs-btn-font-size: .65rem;"> Done </button>
        <button class="btn btn-danger btn-sm" v-else @click="$emit('change-state')"
        style="--bs-btn-padding-y: .25rem; --bs-btn-padding-x: .5rem; --bs-btn-font-size: .65rem;"> To Do </button>
        <p>{{ content }}</p>
    </div>
</template>

<script>
    export default{
        name: "TodoCard",
        emits: ["change-state"],
        data(){
            return{
            }
        },
        props: {
            title: String,
            content: String,
            state: Boolean
        }       
    }
</script>

<style scoped>
    span{
        color: white;
        margin-right: .5rem;
        font-weight: bold;
    }

    p{
        color: white;
        font-size: smaller;
        margin-top: .7rem;
    }
</style>