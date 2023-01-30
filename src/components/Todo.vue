<template>
    <div class="formHolder">
        <form action="">
            <div class="mb-3">
                <label class="form-label" for="title">Title</label>
                <input type="text" class="form-control" name="title" v-model="title" placeholder="Drink some water...">
            </div>

            <div class="mb-3">
                <label class="form-label" for="content">Content</label>
                <input type="text" class="form-control" name="content" v-model="content" placeholder="5 liters in the morning...">
            </div>
            
            <div class="mb-3">
                <label class="form-check-label" for="title">Completed?</label>
                <input class="form-check-input" id="form-checkbox" type="checkbox" name="state" v-model="state"><br>
            </div>

            <button class="btn btn-outline-success" type="submit" @click="addNewTodo">Add</button>
        </form>

    </div>

    <div v-show="todos.length > 0" class="todosHolder">
        <div class="todoCard" v-for="(item, index) in todos" :key="index">
            <div class="close-btn">
                <button class="btn btn-outline-light btn-sm" @click="delTodo(item.id)"
                style="--bs-btn-padding-y: .15rem; --bs-btn-padding-x: .3rem; --bs-btn-font-size: .60rem; margin-top: .7rem; float: right; right: 0;">X</button>
            </div>
            <TodoCard @change-state="changeState(item.id, item.title, item.content, item.state)" :title="item.title" :content="item.content" :state="item.state"/>
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
    padding: 1rem 0;
}

label{
    font-weight: bold;
}

#form-checkbox{
    margin-left: .7rem;
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
    background-color: #01261F;
    margin-top: 1rem;
    padding: 0 .5rem 0 .5rem;
}

.close-btn{
    width: 100%;
    padding: 0;
}

.close-btn button{
    margin-left: 70%;
}

</style>