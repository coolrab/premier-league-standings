<template>
    <div>
        <h1>All Matches</h1>
        <!-- Distributing all the matches played by the team -->
        <v-container>
            <v-card color="#FAFAFA" flat v-for="(matches,i)  in allMatches" :key="matches[i].id">
                <v-layout row wrap>
                    <!-- used flex box for responsiveness for the extra small screen it will take the entire with and display as single row. for the small screen it will take half space and will display in two column and for the medium and above it will display three column -->
                    <v-flex xs12 sm6 md4 class="mx-auto" v-for="match in matches" :key="match.id">
                        <v-card color="#3F1052" dende class="text-center  ma-2">
                            <v-card-text>
                                <div class="subheading grey--text font-weight-bold">{{match.title}}</div>
                            </v-card-text>
                        </v-card>
                    </v-flex>
                </v-layout>
            </v-card>
        </v-container>
    </div>

</template>

<script>
    export default {
        props: ['allMatches'], // allMatches array is receiving as a prop to this component
    }
</script>
