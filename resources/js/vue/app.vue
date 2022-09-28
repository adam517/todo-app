<template>
    <div>
        <v-card class="mx-auto" max-width="250">
            <v-card-text>
                <textarea
                    @keyup.enter="newProject"
                    v-model="projectName"
                    placeholder="Write project name"
                ></textarea>
            </v-card-text>
            <v-card-actions>
                <v-btn color="primary" dark @click="newProject">
                    add new project
                </v-btn>
            </v-card-actions>
        </v-card>

        <div data-app class="flex-container">
            <Project
                @refreshListProjects="projectUpdated"
                v-for="(project, index) in projectsArray"
                :key="index"
                :project="project"
                :index="index"
            ></Project>
        </div>
    </div>
</template>

<script>
import Project from "./Project";

export default {
    data() {
        return {
            projectName: "",
            projectsArray: [],
        };
    },
    mounted() {
        // this.projectUpdated();
    },
    methods: {
        newProject() {
            let projectName = this.projectName;
            let getLocalStorageData = localStorage.getItem("projectsArray");
            if (getLocalStorageData == null) {
                this.projectsArray = [];
            } else {
                this.projectsArray = JSON.parse(getLocalStorageData);
            }
            this.projectsArray.push({ projectName });
            localStorage.setItem(
                "projectsArray",
                JSON.stringify(this.projectsArray)
            );
            this.projectName = "";
        },
        projectUpdated() {
            this.projectsArray = JSON.parse(
                localStorage.getItem("projectsArray")
            );
        },
    },
    name: "App",
    components: { Project },
};
</script>

<style>
/* @import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700&display=swap"); */
.todoList li:hover .icon {
    right: 0px;
}
div {
    direction: ltr;
}
.flex-container {
    display: grid;
    flex-wrap: nowrap;
    /* padding: 10px; */
    /* grid-auto-columns: 400px;
    grid-auto-flow: column; */
    grid-gap: 8px;
    display: grid;
    grid-template-columns: auto auto auto auto auto;
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
.todoList li:hover .icon {
    right: 0px;
}

.todoList li .check {
    position: absolute;
    right: -45px;
    background: #6b706c;
    width: 45px;
    text-align: center;
    color: #fff;
    border-radius: 0 3px 3px 0;
    cursor: pointer;
    transition: all 0.2s ease;
}
.todoList li:hover .check {
    right: 45px;
}

.todoList li .view {
    position: absolute;
    right: -45px;
    background: #3ce7ae;
    width: 45px;
    text-align: center;
    color: #fff;
    border-radius: 0 3px 3px 0;
    cursor: pointer;
    transition: all 0.2s ease;
}
.todoList li:hover .view {
    right: 91px;
}
</style>
