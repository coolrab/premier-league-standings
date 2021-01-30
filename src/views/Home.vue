<template>
    <div>
        <div>
          <!-- Passing  team status data(sports) to the schedules component-->
          <!-- receiving event handler with team id -->
            <schedules :sports="sports" @getMatchData="getTeams"></schedules>

        </div>


        <div>
           <!-- Passing  all the matches played by a team data(allMatches) to the TeamMatches component -->
            <teammatches :allMatches="allMatches">
            </teammatches>  
          </div> 
      </div>

</template>

<script>
    import axios from 'axios'
    import schedules from '@/components/Schedules.vue';
    import teammatches from '@/components/TeamMatches.vue';
    export default {
        components: {
            schedules,
            teammatches,

        },
        data() {
            return {
                sports: [],
                allMatches: [],
                isLoading: false,

            }
        },
        mounted() { //API called for the teams status data  
            let body = {
                // Graphql query for getting  stage tabel
                query: ` query StageTable($stageId: Int!, $sportId: Int!) { table(stageId: $stageId, sportId: $sportId){
                ...tableFragment }
                }
                fragment tableFragment on Table { tableDividingLines {
                typeId fromPlace toPlace type
                id }
                stage { id name yearStart yearEnd
                } teams {
                place
                played
                won
                draw
                lost
                goalsScored goalsConceded goalDifference
                points changeSinceLastRound name
                trimmedName
                id }
                } `,
                variables: {
                    "stageId": 683940,
                    "sportId": 1
                }
            }

            let options = {
                headers: {
                    'Content-Type': 'application/json'
                }
            }

            axios.post('https://sports-api.stage-sumo.tv2.no/graphql', body, options)
                .then((res) => {
                    this.sports = res.data.data.table.teams
                }).catch((err) => {
                    console.log(err)
                })

        },

        methods: {
            async getTeams(id) { //API called for the team matches data 
                //  this.$router.push('/matches')
                this.isLoading = true
                try {
                    const teamId = id
                    console.log(teamId);
                      //Graphql query for getting  team matches
                    let body = {
                        query: ` query StatsTeamMatches($teamId: Int!, $sportId: Int) { allMatchesForTeam(teamId: $teamId, sportId: $sportId) {
                        id
                        name groupName yearStart yearEnd hasTable tournament {
                        id
                        title }
                        matches { ...graphStatsTeamMatch
                        } }
                        }
                        fragment graphStatsTeamMatch on TeamMatch { id
                        matchTypeId title timestamp date
                        stageId homeScore awayScore sportId result {
                        homeScore45 homeScore90 awayScore45 awayScore90
                        } homeTeam {
                        ...teamMatchTeam }
                        awayTeam { ...teamMatchTeam
                        } matchStatusId tvChannels {
                        id
                        nid
                        name
                        sportId
                        progId broadcastTime
                        } }
                        fragment teamMatchTeam on MatchTeam { name
                        id
                        shortName
                        trimmedName } `,
                        variables: {
                            "teamId": teamId,
                            "sportId": 1
                        }
                    }

                    let options = {
                        headers: {
                            'Content-Type': 'application/json'
                        }
                    }

                    await axios.post('https://sports-api.stage-sumo.tv2.no/graphql', body, options)
                        .then((res) => {

                            let teamMaches = res.data.data.allMatchesForTeam
                            // filterd the data with the match.id to get the premier leage data only
                            teamMaches.filter((match) => { 
                                if (match.id === 683940) {
                                    console.log(match.matches);
                                    return this.allMatches.push(match.matches)
                                }

                            })
                            this.$route.push('/matches') // after pushing the data to the child component this will take the user to the match route for showing all the match played by the selected team
                        })
                } catch (error) {
                    console.log(error.message);
                }

                this.isLoading = false
            }
        },
        
    }
</script>