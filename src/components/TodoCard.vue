<template>
    <div>
        <span> {{ title }} </span><br>
        <button v-if="state" @click="$emit('change-state')"> C </button>
        <button v-else @click="$emit('change-state')"> N </button>
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
