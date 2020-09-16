<template>
    <div class="todo-list">
        <input type="text" class="todo-input" v-model="newTodo" @keyup.enter="addTodo"
        placeholder="What needs to be done">
        <div v-for="(todo,index) in todos" :key="todo.id" class="todo-item">
            <div class="todo-item-left">
                <input type="checkbox" v-model="todo.completed">
                <div class="todo-item-label" v-if="!todo.edit"
                @dblclick="editTodo(todo)" :class="{completed: todo.completed}">
                    {{todo.title}}
                </div>
                <input v-else class="todo-item-edit" type="text"
                v-model="todo.title" @blur="doneEdit(todo)"
                @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)"
                v-focus>
            </div>
            <div class="remove-item" @click="removeTodo(index)">
                &times;
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name:'todo-list',
    data(){
        return {
            newTodo: '',
            idTodo: 3,
            beforeEditTitle: '',
            todos: [
                {
                    'id': 1,
                    'title': 'Finish Vue Screencast',
                    'completed': false,
                    'edit': false
                },
                {
                    'id': 2,
                    'title': 'Take over world',
                    'completed': false,
                    'edit': false
                }
            ]
        }
    },
    directives: {
        focus: {
            inserted: function(el){
                el.focus()
            }
        }
    },
    methods: {
        addTodo() {
            if(this.newTodo.trim().length == 0 ) {
                return
            }
            this.todos.push({
                id: this.idTodo,
                title: this.newTodo,
                completed: false,
            })
            this.newTodo = '',
            this.idTodo++
        },
        editTodo(todo){
            this.beforeEditTitle = todo.title
            todo.edit = true
        },
        doneEdit(todo){
            if(todo.title.trim().length == 0 ) {
                todo.title = this.beforeEditTitle
            }
            todo.edit = false
        },
        cancelEdit(todo){
            todo.title = this.beforeEditTitle
            todo.edit = false
        },
        removeTodo(index) {
            this.todos.splice(index, 1)
        }
    }
}
</script>

<style lang="scss" scoped>
    .todo-input {
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;

        &:focus {
            outline: 0;
        }
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .remove-item {
        cursor: pointer;
        margin-left: 15px;
        &:hover{
            color: black;
        }
    }

    .todo-item-left {
        display: flex;
        align-items: center;
    }

    .todo-item-label{
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }
    .todo-item-edit{
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;

        &:focus{
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        color: grey;
    }
</style>