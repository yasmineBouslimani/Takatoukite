<template>
    <section class="interventionsapp">
        <header class="header">
            <h1>Takatoukité</h1>
            <input type="text" class="new-intervention" placeholder="Ajouter une demande d'intervention"
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
                    <!--                TODO voir la fonctionnalité @blur pour doneEditIntervention en cliquant ailleurs -->
                    <input type="text" class="edit" v-model="intervention.name" @keyup.enter="doneEditIntervention">
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
                    completed: false
                }],
                newIntervention: "",
                editing: null
            }
        },
        methods: {
            addInterventions() {
                this.interventionsList.push({
                    completed: false,
                    name: this.newIntervention
                })
                this.newIntervention = ''
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
