<template>
<div>
    <v-row>
        <v-col class="mx-auto mt-5">
            <v-text-field @keyup.enter="addTask()" @click:append="addTask()" v-model="newTask" label="Nouvelle tâche" :rules="rules" solo clearable append-icon="mdi-plus">
            </v-text-field>
        </v-col>
    </v-row>
    <v-row>
        <v-col class="mx-auto">
            <v-card>
                <v-row justify="between" class="px-5">
                    <v-col>
                        <p>Liste de Tâches</p>
                    </v-col>
                    <v-col v-if="totalTask">
                        <p class="text-right">Total tâches: {{totalTask}}</p>
                    </v-col>
                </v-row>

                <v-slide-y-transition>
                    <v-divider v-if="totalTask"></v-divider>

                </v-slide-y-transition>
                <v-slide-y-transition>
                    <v-row class="my-1 px-5" align="center" v-if="totalTask">
                        <strong class="mx-4 info--text text--darken-2">
                            Tâches restantes: {{ remainingTasks }}
                        </strong>
                        <v-divider vertical></v-divider>
                        <strong class="mx-4 success--text text--darken-2">
                            Tâches finies: {{ completedTasks }}
                        </strong>
                        <v-spacer></v-spacer>
                        <v-progress-circular :value="progress" class="mr-2" color="green"></v-progress-circular>
                    </v-row>

                </v-slide-y-transition>
                <v-slide-y-transition>
                    <v-divider v-if="totalTask"></v-divider>
                </v-slide-y-transition>
                <v-slide-y-transition>
                    <v-list v-if="!searchString">
                        <v-slide-y-transition group leave-absolute>
                            <div v-for="task in tasks" :key="task.id">
                                <v-list-item @click="task.fini = !task.fini" :class="task.fini ? 'primary lighten-5':''">

                                    <v-list-item-action>
                                        <v-checkbox :input-value="task.fini"></v-checkbox>
                                    </v-list-item-action>
                                    <v-list-item-content>
                                        <v-list-item-title :class="{'text-decoration-line-through disabled' : task.fini}">{{task.titre}} </v-list-item-title>
                                    </v-list-item-content>

                                    <v-list-item-action>
                                        <v-btn icon @click="removeTask(task.id)">
                                            <v-icon color="grey lighten-1">mdi-delete</v-icon>
                                        </v-btn>
                                    </v-list-item-action>
                                    <v-list-item-action>
                                        <v-btn icon @click="task.fav = !task.fav">

                                            <v-icon color="red darken-1" v-if="task.fav">mdi-heart</v-icon>
                                            <v-icon color="grey lighten-1" v-else>mdi-heart-outline</v-icon>
                                        </v-btn>
                                    </v-list-item-action>
                                    <v-scroll-x-transition>
                                        <v-icon v-if="task.fini" color="success">
                                            mdi-check
                                        </v-icon>
                                    </v-scroll-x-transition>

                                </v-list-item>
                                <v-divider></v-divider>
                            </div>
                        </v-slide-y-transition>

                    </v-list>
                    <v-list v-else>
                        <v-slide-y-transition group leave-absolute>
                            <div v-for="task in searchResult" :key="task.id">
                                <v-list-item @click="task.fini = !task.fini" :class="task.fini ? 'primary lighten-5':''">
                                    <template v-slot:default="{ active }">
                                        <v-list-item-action>
                                            <v-checkbox :input-value="active"></v-checkbox>
                                        </v-list-item-action>
                                        <v-list-item-content>
                                            <v-list-item-title :class="{'text-decoration-line-through disabled' : task.fini}">{{task.titre}} </v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                            <v-btn icon @click="removeTask(task.id)">
                                                <v-icon color="red lighten-1">mdi-delete</v-icon>
                                            </v-btn>
                                        </v-list-item-action>
                                        <v-scroll-x-transition>
                                            <v-icon v-if="task.fini" color="success">
                                                mdi-check
                                            </v-icon>
                                        </v-scroll-x-transition>
                                    </template>
                                </v-list-item>
                                <v-divider></v-divider>
                            </div>
                        </v-slide-y-transition>

                    </v-list>
                </v-slide-y-transition>
            </v-card>
        </v-col>
    </v-row>
</div>
</template>

<script>
export default {
    name: "Home",
    props: {

        searchString: {
            type: String,
            default: null
        }
    },
    data() {
        return {
            newTask: null,
            rules: [
                v => v.length <= 75 || 'Maximum 75 caractères',
                v => v.length > 1 || 'Au moins 2 caractères',

            ],
            tasks: [],
            searchResult: []
        }

    },
    methods: {
        makeSearch() {
            if (this.searchString.trim()) {
                this.searchResult = this.tasks.filter(task => task.titre.includes(this.searchString))
            } 
            else {
                this.searchResult = this.tasks
            }

        },
        removeTask(id) {
            console.log("remove")
            this.tasks = this.tasks.filter(task => task.id !== id)
        },
        addTask() {
            if (this.newTask) {

                let n = 1
                while (this.tasks.filter(task => task.id == this.totalTask + n).length) {
                    n++
                }
                this.tasks.push({
                    id: this.totalTask + n,
                    titre: this.newTask,
                    fini: false,
                    fav: false
                })
                this.newTask = null
                console.log(this.tasks)
            }

        }
    },
    computed: {
        totalTask() {
            return this.tasks.length
        },
        completedTasks() {
            return this.tasks.filter(task => task.fini === true).length
        },
        remainingTasks() {
            return this.tasks.length - this.completedTasks
        },
        progress() {
            return this.completedTasks / this.totalTask * 100
        }
    },
    watch: {
        searchString() {

            this.makeSearch()

            console.log(this.searchString)
        }
    }
};
</script>
