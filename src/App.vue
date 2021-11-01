<template>
<v-app>

    <v-main class="grey lighten-4">
        <v-container >
            <navbar @searching="searching = $event" @sendString="searchString = $event" />

            <router-view :searching="searching" :searchString="searchString" />

        </v-container>
    </v-main>
</v-app>
</template>

<script>
import Navbar from '@/components/Navbar.vue';

export default {
    name: "App",

    data: () => ({
        searching : false,
        searchString: null
    }),
    components: {
        Navbar
    },
    watch: {
        searchString(){
            console.log(this.searchString)
        }
    }
};
</script>
