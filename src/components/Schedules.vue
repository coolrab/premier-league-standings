<template>
    <v-card>
        <!-- Team standings  -->

        <v-simple-table fixed-header height="850px">
            <template v-slot:default>
                <thead>
                    <tr>
                        <th class="text-left">
                            Position
                        </th>
                        <th class="text-left">
                            Club
                        </th>
                        <th class="text-left">
                            MP
                        </th>
                        <th class="text-left">
                            W
                        </th>
                        <th class="text-left">
                            D
                        </th>
                        <th class="text-left">
                            L
                        </th>
                        <th class="text-left">
                            GS
                        </th>
                        <th class="text-left">
                            GC
                        </th>
                        <th class="text-left">
                            GD
                        </th>
                        <th class="text-left">
                            Pts
                        </th>
                    </tr>
                </thead>
                <tbody>

                    <!-- loop through the matches aprray and displayed the data also make a clik event with the clicked team id and pass it to the parent component for calling data for the all the matches played by that team -->
                    <tr v-for="sport in sports" :key="sport.id" :loading="isLoading" @click="getMatchData(sport.id)">
                        <td>{{ sport.place}}</td>
                        <td>{{ sport.name}}</td>
                        <td>{{ sport.played}}</td>
                        <td>{{ sport.won}}</td>
                        <td>{{ sport.draw}}</td>
                        <td>{{ sport.lost}}</td>
                        <td>{{ sport.goalsScored}}</td>
                        <td>{{sport.goalsConceded}}</td>
                        <td>{{sport.goalDifference}}</td>
                        <td>{{sport.points}}</td>

                    </tr>
                </tbody>
            </template>
        </v-simple-table>
    </v-card>
</template>

<script>
    export default {

        props: ["sports"], //Data props reveived from the parent component
        data() {
            return {
                isLoading: false,

            }
        },
        methods: {
            getMatchData(id) { // emeting an event to the parent component with the team id
                this.$emit('getMatchData', id, this.isLoading)

            }
        }

    }
</script>