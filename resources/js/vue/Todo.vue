<template>
    <div>
        <span class="view">
            <div class="text-center">
                <v-dialog v-model="dialog" width="500">
                    <template v-slot:activator="{ on, attrs }">
                        <i
                            class="fa-sharp fa-solid fa-eye"
                            color="red lighten-2"
                            dark
                            v-bind="attrs"
                            v-on="on"
                            @click="getDescription"
                        >
                        </i>
                    </template>

                    <v-card>
                        <v-card-title class="text-h5 grey lighten-2">
                            Todo description. Times viewed -
                            {{ todoItem.viewNumber }}
                        </v-card-title>

                        <v-text-field
                            v-model="todoItem.description"
                            label="Write description"
                            outlined
                        >
                        </v-text-field>

                        <v-divider></v-divider>

                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="primary" text @click="handleOk">
                                ok
                            </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </div>
        </span>
        <!-- Log In form -->

        <span class="icon" @click="deleteTask(index)"
            ><i class="fas fa-trash"></i>
        </span>
        <span class="check" @click="taskDone(index)">
            <i
                v-if="todoItem.taskDone === false"
                class="fa-sharp fa-solid fa-square"
            >
            </i>
            <i v-else class="fa-sharp fa-solid fa-square-check"></i>
        </span>
        {{ todoItem.userEnteredValue }}
    </div>
</template>

<script>
export default {
    props: ["todoItem", "index", "project"],

    data() {
        return {
            listTodo: [],
            description: "",
            dialog: false,
            viewNumber: 0,
        };
    },
    methods: {
        getDescription() {
            this.todoItem.viewNumber++;
        },
        taskDone(index) {
            this.getLocalTodoList();
            this.listTodo[index].taskDone = !this.listTodo[index].taskDone;
            this.updateList();
        },
        deleteTask(index) {
            this.getLocalTodoList();
            this.listTodo.splice(index, 1);
            this.updateList();
        },

        getLocalTodoList() {
            let getLocalStorageData = localStorage.getItem(
                this.project.projectName
            );
            this.listTodo = JSON.parse(getLocalStorageData);
        },

        updateList() {
            localStorage.setItem(
                this.project.projectName,
                JSON.stringify(this.listTodo)
            );
            this.$emit("refreshList");
        },

        handleOk() {
            // Prevent modal from closing
            this.viewNumber = this.todoItem.viewNumber;
            this.description = this.todoItem.description;
            this.getLocalTodoList();
            this.listTodo[this.index].description = this.description;
            this.listTodo[this.index].viewNumber = this.viewNumber;
            this.updateList();
            this.dialog = false;
        },
    },
};
</script>
<style></style>
