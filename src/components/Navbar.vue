<template>
<div>

    <v-app-bar color="grey lighten-4 pa-0" dense flat>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

        <v-toolbar-title>Sage Todo</v-toolbar-title>

        <v-spacer></v-spacer>

        <div class="mt-8">
            <v-scroll-y-transition>
                <v-text-field v-if="searching" @keyup="sendSearchString()" @click:append="sendSearchString()" v-model="searchString" label="Chercher une tâche" :rules="rules" solo clearable append-icon="mdi-magnify" class="my-1">
                </v-text-field>
            </v-scroll-y-transition>

        </div>

        <v-btn icon @click="searching = !searching; $emit('searching', searching)">
            <v-icon>mdi-magnify</v-icon>
        </v-btn>

    </v-app-bar>
    <v-navigation-drawer v-model="drawer" app>
        <v-list-item>
            <v-list-item-content>
                <v-list-item-title class="text-h6">
                    Sage Todo
                </v-list-item-title>
                <v-list-item-subtitle>
                    gestionnaire de tâche
                </v-list-item-subtitle>
            </v-list-item-content>
        </v-list-item>

        <v-divider></v-divider>

        <v-list dense nav>
            <v-list-item v-for="item in items" :key="item.title" link :to="item.route">
                <v-list-item-icon>
                    <v-icon>{{ item.icon }}</v-icon>
                </v-list-item-icon>

                <v-list-item-content>
                    <v-list-item-title>{{ item.name }}</v-list-item-title>
                </v-list-item-content>
            </v-list-item>
        </v-list>
    </v-navigation-drawer>

</div>
</template>

<script>
export default {
    name: "HelloWorld",

    data: () => ({
        drawer: false,
        searching: false,
        searchString: null,
        items: [{
                name: "Tâches",
                icon: "mdi-format-list-checks",
                route: '/'
            },
            {
                name: "A propos",
                icon: "mdi-help",
                route: "/about"
            },
        ]
    }),
    methods: {
        sendSearchString() {
            this.$emit('sendString', this.searchString)
        }
    },
    watch: {
        searchString() {
            setTimeout(()=>{
                this.searchString = null
                this.sendSearchString()
            }, 5000)
        }
    }
};
</script>
