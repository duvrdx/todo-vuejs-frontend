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
            <TodoCard @change-state="changeState(index)" :title="item.title" :content="item.content" :state="item.state"/>

            <button @click="delTodo(item.id)">Delete</button>
        </div>
    </div>
</template>

<script>
import TodoCard from "./TodoCard.vue"
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
            todos: [
            ]
        }
    },
    methods:{
        addNewTodo(e){
            e.preventDefault()

            this.todos.push(
                {
                    title: this.title,
                    content: this.content,
                    state: this.state 
                }
            )

            this.title = ""
            this.content = ""
            this.state = false
        },

        delTodo(id){
            this.todos.splice(id,1)
        },

        changeState(id){
            this.todos[id].state = !this.todos[id].state
            console.log(this.todos[id].state, id)
        }
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