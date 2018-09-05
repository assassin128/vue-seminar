<template>
    <div>
        <input type="text" placeholder="add new todo" v-model.trim="todo" @keyup.enter="addTodo">
        <ul>
            <li :class="{done: todo.status, edit: editedTodo && editedTodo.id === todo.id}" v-for="todo in todos" :key="todo.id">
                <span @dblclick="editTodo(todo)">{{todo.name}}</span>
                <button @click="removeTodo(todo)">remove</button>
                <button @click="changeStateTodo(todo)">change state</button>
                <input type="text" v-model.trim="todo.name" v-todo-focus="todo === editedTodo" @blur="completeEdit(todo)" @keyup.enter="completeEdit(todo)" @keyup.esc="escEdit(todo)">
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    name: "Todo",
    data() {
        return {
            todos: [
                // { id: 1, name: "Todo 1", state: false }
            ],
            todo: null,
            editedTodo: null,
            editName: ""
        };
    },
    methods: {
        addTodo() {
            if (!this.todo) {
                return;
            }
            this.todos.push({ id: this.todos.length + 1, name: this.todo, status: false });
            this.todo = "";
        },
        removeTodo(todo) {
            this.todos.splice(this.todos.indexOf(todo), 1);
        },
        changeStateTodo(todo) {
            todo.status = !todo.status;
        },
        editTodo(todo) {
            this.editedTodo = todo;
            this.editName = todo.name;
        },
        completeEdit(todo) {
            this.editmode = false;
            if (!this.editedTodo) {
                return;
            }
            this.editedTodo = null;
            if (!todo.name) {
                this.removeTodo(todo);
            }
        },
        escEdit(todo) {
            this.editedTodo = null;
            todo.name = this.editName;
        }
    },
    directives: {
        "todo-focus": function(el, binding) {
            if (binding.value) {
                el.focus();
            }
        }
    }
};
</script>
<style>
ul li {
    list-style: none;
    position: relative;
}
ul {
    width: 500px;
    margin: 20px auto;
}
.done span {
    text-decoration: line-through;
}
li input {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    display: none;
}
li.edit input {
    display: block;
}
li span {
    display: inline-block;
    padding: 0 20px;
    width: 100%;
    margin-bottom: 5px;
}
</style>
