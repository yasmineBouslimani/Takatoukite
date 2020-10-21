<template>
    <section class="interventionsapp">
        <header class="header">
            <h1>Takatoukité</h1>
            <input type="text" class="new-intervention" placeholder="Ajoutez votre demande d'intervention"
                   v-model="newIntervention" v-on:keyup.enter="addInterventions">
        </header>
        <div class="main">
            <ul class="interventions-list">
                <li class="intervention" v-for="intervention in interventionsList" :key="intervention.id"
                    :class="{completed: intervention.completed, editing: intervention === editing}">
                    <div class="view">
                        <input type="checkbox" v-model="intervention.completed" class="toggle">
                        <label @dblclick="editInterventions(intervention)">{{intervention.name}}</label>
                    </div>
                    <input type="text" class="edit" v-model="intervention.name" @keyup.enter="doneEditIntervention">
                    <div class="moreInformations" @click="showModal = true"></div>
                    <div class="modal is-active" v-if="showModal" @click="showModal = false">
                        <div class="modal-background"></div>
                        <div class="modal-content">
                            <div class="box">
                                <p style="font-size: 20px; font-weight: bold">{{intervention.name}}</p>
                                <br>
                                <p style="font-size: 18px">" {{intervention.details}} "</p>
                                <br>
                                <p style="font-size: 12px">{{intervention.date}}</p>
                            </div>
                        </div>
                    </div>
                </li>
            </ul>

        </div>
        <footer class="footer">
            <span class="interventions-count"><strong>{{ remainingInterventions }}</strong> intervention(s) à traiter</span>
        </footer>
    </section>
</template>
<script>
    export default {
        data() {
            return {
                interventionsList: [{
                    name: "Impossible de télécharger le pdf",
                    details: "I'm speaking with myself, number one, because I have a very good brain and I've said a lot of things. I know words. I have the best words.",
                    completed: false,
                    date: new Date().toLocaleString()
                }],
                newIntervention: "",
                editing: null,
                showModal: false
            }
        },
        methods: {
            addInterventions() {
                this.interventionsList.push({
                    name: this.newIntervention,
                    details: "I was going to say something extremely rough to Lorem Ipsum, to its family, and I said to myself, \"I can't do it. I just can't do it. It's inappropriate. It's not nice.\" That other text? Sadly, it’s no longer a 10.",
                    completed: false,
                    date: new Date().toLocaleString()
                })
            },
            editInterventions(intervention) {
                this.editing = intervention
            },
            doneEditIntervention() {
                this.editing = null
            }

        },
        computed: {
            remainingInterventions() {
                return this.interventionsList.filter(intervention => !intervention.completed).length
            }
        }

    }
</script>

<style src="./interventions.css"></style>
