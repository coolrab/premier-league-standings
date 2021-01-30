  
<template>
    <v-app id="inspire">
        <!-- App bar with the logo -->
        <v-app-bar app color="#3F1052">
            <v-app-bar-icon @click="goHome">
                <v-avatar>
                    <img src="epl.png" alt="English Premier League">
                </v-avatar>
            </v-app-bar-icon>
              <!-- App bar with the title -->
            <v-toolbar-title class="flex text-center pt-4 pb-1">
                <h3 class="subheading grey--text">Premier League Standings</h3>
                <p class="font-weight-regular grey--text">Season 2020-21</p>
            </v-toolbar-title>
        </v-app-bar>

        <v-main>

            <v-container fluid>
              <!-- Vue router view -->
                <router-view></router-view>

            </v-container>
        </v-main>
    </v-app>
</template>

<script>
    export default {
        name: 'App',

        components: {

        },

        data: () => ({
            drawer: null
        }),
        methods: {
            goHome() { //@click on the logo this method will directed to the home page 
                this.$router.push('/')
            }
        }


    };
</script>