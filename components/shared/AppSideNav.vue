<template>
    <div class="side-nav">
        <form action="" class="form" @submit.prevent="addMember">
            <input type="text" class="input input-top" placeholder="Member Name" v-model="name" required>
            <input type="email" class="input" placeholder="Email" v-model="email" required>
            <input type="text" class="input" placeholder="Role" v-model="role" required>
            <input type="submit" class="input input-submit" value="Add Member">
        </form>
        <div class="members-container">
            <h4 class="members" v-for="member in members" :key="member.id" :id="member.id" @click="changeRoute(member.id)">
                {{member.name}}
            </h4>
        </div>
        <form class="generate-container" @submit.prevent="generateTeam">
            <input type="number" class="input input-number" placeholder="Number of members in team" required v-model="membersPerTeam">
            <input type="submit" class="generate-btn" value="Generate Team">
        </form>
    </div>
</template>

<script>
   import {mapState, mapMutations} from "vuex"
   
    export default {
        data(){
            return {
                name: "",
                email: "",
                role: "",
                membersPerTeam: ""
            }
        },
        computed : {
            ...mapState({
                members: state => state.members,
                teams: state => state.teams
            })
        },
        methods: {
            ...mapMutations({
                addUser: "addUser",
                addTeams: "addTeams"
            }),
            generateTeam(){
                if(this.members.length < parseInt(this.membersPerTeam)){
                    return
                }
                let teams = [];
                let members = [...this.members];

                while(members.length){
                    let team = []
                    for(let i = 0; i < this.membersPerTeam; i++){
                        console.log("BEFORE", members)
                        let index = Math.floor(Math.random() * members.length)
                        team.push(members.splice(index, 1)[0])
                        console.log("AFTER", members)
                        if(members.length === 0) break
                    }
                    teams.push(team)
                }

                this.addTeams(teams)
            },
            addMember(){
                this.addUser({
                    name: this.name,
                    email: this.email,
                    role: this.role
                })
                this.name = "";
                this.email = "";
                this.role = ""
            },
            changeRoute(userId){
                this.$router.push(`/generator/${userId}`)
            } 
        }
    }
</script>

<style scoped>
    .side-nav {
        width: 30rem;
        height: 90vh;
        background-color: rgba(124, 124, 120, 0.192);

        
    }

    /* FORM STYLING */
    .form {
        display: flex;
        flex-direction: column;
        width: 75%;
        margin: 0 auto;
    }

    .input {
        margin: 0.25rem;
        padding: 0.5rem;
        border-radius: 0.2rem;
    }

    .input-top {
        margin-top: 5rem
    }

    .input-submit {
        background-color: rgba(50, 230, 26, 0.295);
        cursor: pointer
    }

    /* Members container */
    .members-container {
        width: 75%;
        margin: 1rem auto;
        height: 30vh;   
        overflow-y: auto;
        align-items: flex-start;
    }

    .members {
        padding: 0.5rem;
        background-color: rgba(0, 0, 0, 0.507);
        height: 1.5rem;
        border-radius: 0.2rem;
        color: white;
        font-weight: 400;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        margin: 0.5rem;
        float: left
    }

    /* Button styling */

    .generate-container {
        width: 75%;
        margin: 0 auto
    }

    .generate-btn {
        background-color: rgba(0, 0, 0, 0.459);
        padding: 0.5rem;
        width: 100%
    }

    .input-number {
        width: 100%;
        margin-bottom: 1rem;
        text-align: center;
    }


</style>