<template>
    <div class="formHolder">
        <form action="">
            <label for="title">Title</label><br>
            <input type="text" name="title" v-model="title"><br>
            <label for="title">Description</label><br>
            <input type="text" name="content" v-model="content"><br>
            <label for="title">Completed?</label>
            <input id="form-checkbox" type="checkbox" name="state" v-model="state"><br>

            <button type="submit" @click="addNewTodo">Add</button>
        </form>

    </div>

    <div v-show="todos.length > 0" class="todosHolder">
        <div class="todoCard" v-for="(item, index) in todos" :key="index">
            <TodoCard @change-state="changeState(item.id, item.title, item.content, item.state)" :title="item.title" :content="item.content" :state="item.state"/>
            <button @click="delTodo(item.id)">Delete</button>
        </div>
    </div>
</template>

<script>
import TodoCard from "./TodoCard.vue"
import api from "../services/api.js"

export default{
    name: "Todo",
    components: {
        TodoCard
    },
    data(){
        return{
            title: "",
            content: "",
            state: false,
            todos: []
        }
    },
    methods:{
        addNewTodo(e){
            e.preventDefault()
            if(this.title === ""){
                const a = confirm("Title is empty")
                if(a){
                    this.title = " "
                }else{
                    this.title = prompt("Insert a title:")
                }
            }

            if(this.content === ""){
                const b = confirm("Content is empty")
                if(b){
                    this.content = " "
                }else{
                    this.content = prompt("Insert a content:")
                }
            }

            api.post("todo/add/" + this.title + "/" + this.content + "/" + this.state).then(() => {
                api.get().then((response) => {
                const todos = response["data"]
                this.todos = []
                for(const item in todos){
                    api.get("todo/read/"+todos[item]).then((response) => {
                        this.todos.push(response["data"])
                    })
                };})
            })
        },

        delTodo(id){
            api.get("todo/delete/" + id).then(() =>{
                api.get().then((response) => {
                const todos = response["data"]
                this.todos = []
                for(const item in todos){
                    api.get("todo/read/"+todos[item]).then((response) => {
                        this.todos.push(response["data"])
                    })
                };})
            })
        },

        changeState(id, title, content, state){
            api.post("todo/update/" + id + "/" + title + "/" + content + "/" + !state).then(()=>{
                api.get().then((response) => {
                const todos = response["data"]
                this.todos = []
                for(const item in todos){
                    api.get("todo/read/"+todos[item]).then((response) => {
                        this.todos.push(response["data"])
                    })
                };})
            })
        }

    },created(){
        api.get().then((response) => {
                const todos = response["data"]
                this.todos = []
                for(const item in todos){
                    api.get("todo/read/"+todos[item]).then((response) => {
                        this.todos.push(response["data"])
                    })
                };})
    }
}
</script>

<style scoped>
.formHolder{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 70%;
    height: 100%;
    border-radius: 10px;
    background-color: white;
    padding: .5rem;
}

label, input{
    margin: .7rem 0 ;
}

label{
    font-weight: bold;
}

#form-checkbox{
    margin-left: .7rem;
}

button {
    margin-top: 1rem;
}

.todosHolder{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 70%;
}

.todoCard{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    border-radius: 10px;
    background-color: grey;
    margin-top: 1rem;
    padding: .5rem;
}
</style>