<script>
export default {
    data() {
        return {
            count: 0,
            todos: [],
            name: '',
            input_content: '',
            input_category: '',
        }
    },
    
    computed: {
        todos_asc() {
            return this.todos.sort((a, b) => b.createdAt - a.createdAt)
        },
        
    },
    
    mounted() {
        this.name = localStorage.getItem('name') || '';
        this.todos = JSON.parse(localStorage.getItem('todos')) || [];
    },
    
    watch: {
        name(newVal, oldVal) {
            localStorage.setItem('name', newVal);
        },
        
        todos: {
            handler(newVal, oldVal) {
                localStorage.setItem('todos', JSON.stringify(newVal));
            },
            deep: true,
        },
    },
    
    methods: {
        addToDo() {
            if (this.input_content.trim() === '' || this.input_category === '') return;
            
            this.todos.push({
                content: this.input_content,
                category: this.input_category,
                createdAt: new Date().getTime(),
                done: false,
            })
            
            this.input_content = ''
            this.input_category = ''
        },
        
        removeTodo(todo) {
            this.todos = this.todos.filter(t => t != todo)
        }
    }
}

</script>

<template>
    <main class="app">
        <section class="greeting">
            <h2 class="title">
                What's up, <input type="text" placeholder="Name here" v-model="name">
            </h2>
        </section>
        <section class="create-todo">
            <h3>CREATE A TODO</h3>
            <form @submit.prevent="addToDo">
                <h4>What's on your todo list?</h4>
                <input
                    type="text"
                    placeholder="e.g. make a video"
                    v-model="input_content" />
                    
                    <h4>Pick a category</h4>
                    
                    <div class="options">
                        <label>
                            <input
                                type="radio"
                                name="category"
                                value="business"
                                v-model="input_category">
                            <span class="bubble business"></span>
                            <div>Business</div>
                        </label>
                        
                        <label>
                            <input
                                type="radio"
                                name="category"
                                value="personal"
                                v-model="input_category">
                            <span class="bubble personal"></span>
                            <div>Personal</div>
                        </label>
                    </div>
                    <input type="submit" value="Add todo">
            </form>
        </section>
        
        <section class="todo-list">
            <h3>TODO LIST</h3>
            <div class="list">
                <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
                    <label>
                        <input type="checkbox" v-model="todo.done">
                        <span :class="`bubble ${todo.category}`"></span>
                    </label>
                    
                    <div class="todo-content">
                        <input type="text" v-model="todo.content">
                    </div>
                    
                    <div class="actions">
                        <button class="delete" @click="removeTodo(todo)">Delete</button>
                    </div>
                </div>
            </div>
        </section>
    </main>
</template>