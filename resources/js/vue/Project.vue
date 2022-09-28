<template>
    <div class="wrapper">
        <i id="delete" class="fa-solid fa-x" @click="deleteProject"></i>
        <header>{{ project.projectName }}</header>
        <div class="inputField">
            <input
                type="text"
                placeholder="Add your new todo"
                v-model="todoText"
            />
            <button :class="{ active: todoConfirm }" @click="addTodo">
                <i class="fas fa-plus"></i>
            </button>
        </div>

        <ul class="todoList">
            <li v-for="(element, index) in listTodo" :key="index">
                <Todo
                    @refreshList="listUpdated"
                    :todoItem="element"
                    :index="index"
                    :project="project"
                ></Todo>
            </li>
        </ul>
        <div class="footer">
            <span>You have {{ listTodo.length }} pending Todo</span>
        </div>
    </div>
</template>

<script>
import Todo from "./Todo.vue";

export default {
    props: ["project", "index"],
    data() {
        return {
            todoText: "",
            todoConfirm: false,
            listTodo: [],
        };
    },
    mounted() {
        // this.listUpdated();
    },
    watch: {
        todoText() {
            if (this.todoText.length > 0) this.todoConfirm = true;
            else this.todoConfirm = false;
        },
    },
    methods: {
        deleteProject() {
            this.listUpdated();
            this.listTodo = [];
            window.localStorage.removeItem(this.project.projectName);
            let getLocalStorage = JSON.parse(
                localStorage.getItem("projectsArray")
            );
            getLocalStorage.splice(this.index, 1);
            localStorage.setItem(
                "projectsArray",
                JSON.stringify(getLocalStorage)
            );
            this.$emit("refreshListProjects");
        },
        addTodo() {
            let userEnteredValue = this.todoText;
            let getLocalStorageData = localStorage.getItem(
                this.project.projectName
            );
            if (getLocalStorageData == null) {
                this.listTodo = [];
            } else {
                this.listTodo = JSON.parse(getLocalStorageData);
            }
            this.listTodo.push({
                userEnteredValue,
                taskDone: false,
                viewNumber: 0,
                description: "",
            });
            localStorage.setItem(
                this.project.projectName,
                JSON.stringify(this.listTodo)
            );
            this.todoText = "";
        },
        listUpdated() {
            let getLocalStorageData = localStorage.getItem(
                this.project.projectName
            );
            this.listTodo = JSON.parse(getLocalStorageData);
        },
    },
    name: "App",
    components: { Todo },
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    font-family: "Poppins", sans-serif;
}
#delete {
    position: absolute;
    text-align: left;
    top: 5px;
    right: 5px;
}
::selection {
    color: #ffff;
    background: rgb(142, 73, 232);
}
body {
    width: 100%;
    height: 100vh;

    background: linear-gradient(to bottom, #99aca7 0%, #2fb46b 100%);
}
.wrapper {
    position: relative;
    border: 1px solid rgba(0, 0, 0, 0.8);
    padding: 20px;
    font-size: 30px;
    text-align: center;
    background: #fff;
    max-width: 400px;
    width: 100%;
    margin: 120px auto;
    padding: 25px;
    border-radius: 5px;
    box-shadow: 0px 10px 15px rgba(0, 0, 0, 0.1);
}
.wrapper header {
    font-size: 30px;
    font-weight: 600;
}
.wrapper .inputField {
    margin: 20px 0;
    width: 100%;
    display: flex;
    height: 45px;
}
.inputField input {
    width: 85%;
    height: 100%;
    outline: none;
    border-radius: 3px;
    border: 1px solid #ccc;
    font-size: 17px;
    padding-left: 15px;
    transition: all 0.3s ease;
}
.inputField input:focus {
    border-color: #8e49e8;
}
.inputField button {
    width: 50px;
    height: 100%;
    border: none;
    color: #fff;
    margin-left: 5px;
    font-size: 21px;
    outline: none;
    background: #8e49e8;
    cursor: pointer;
    border-radius: 3px;
    opacity: 0.6;
    pointer-events: none;
    transition: all 0.3s ease;
}
.inputField button:hover,
.footer button:hover {
    background: #721ce3;
}
.inputField button.active {
    opacity: 1;
    pointer-events: auto;
}
.wrapper .todoList {
    max-height: 200px;
    height: 200px;
    overflow-y: auto;
}
.todoList li {
    position: relative;
    list-style: none;
    height: 45px;
    line-height: 45px;
    margin-bottom: 8px;
    background: #f2f2f2;
    border-radius: 3px;
    padding: 0 15px;
    cursor: default;
    overflow: hidden;
}
.todoList li .icon {
    position: absolute;
    right: -45px;
    background: #e74c3c;
    width: 45px;
    text-align: center;
    color: #fff;
    border-radius: 0 3px 3px 0;
    cursor: pointer;
    transition: all 0.2s ease;
}
.todoList li .icon {
    right: 0px;
}

.wrapper .footer {
    display: flex;
    width: 100%;
    margin-top: 20px;
    align-items: center;
    justify-content: space-between;
}
.footer button {
    padding: 6px 10px;
    border-radius: 3px;
    border: none;
    outline: none;
    color: #fff;
    font-weight: 400;
    font-size: 16px;
    margin-left: 5px;
    background: #8e49e8;
    cursor: pointer;
    user-select: none;
    opacity: 0.6;
    pointer-events: none;
    transition: all 0.3s ease;
}
.footer button.active {
    opacity: 1;
    pointer-events: auto;
}

.button {
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
}

.button1 {
    background-color: #4caf50;
}
</style>
