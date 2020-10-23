<template>
    <section class="interventionsapp">
        <header class="header">
            <h1>Takatoukité</h1>
            <form
                    id="form"
                    @submit.prevent="checkForm"
                    method="post"
            >
                <div v-if="errors.length" class="alert alert-danger" role="alert">
                    Veuillez remplir le(s) champ(s) suivant(s) :
                    <i v-for="(error, index) in errors" :key="index">{{ error }}</i>
                </div>
                <div class="form-row align-items-center" style="padding: 20px 0 0 20px;">
                    <div class="col-auto">
                        <label class="sr-only">Prénom</label>
                        <div class="input-group mb-2">
                            <div class="input-group-prepend">
                                <div class="input-group-text"><i class="fas fa-user"></i></div>
                            </div>
                            <input
                                    type="text"
                                    placeholder="Quel est votre prénom ?"
                                    v-model="name"
                                    class="form-control"
                            />
                        </div>
                    </div>
                    <div class="col-auto">
                        <label class="sr-only">Détails</label>
                        <input
                                type="text"
                                placeholder="Quel est l'objet de votre demande ?"
                                v-model="details"
                                class="form-control mb-2"
                                style="width: 260px"
                        />
                    </div>
                    <div class="col-auto">
                        <button id="submit" type="submit" class="btn btn-primary mb-2">Envoyer !</button>
                    </div>
                </div>
            </form>
        </header>

        <div class="mt-5 alert alert-success" role="alert" v-if="interventionsListIsEmpty">
            <p class="text-center">Pas d'intervention pour le moment</p>
        </div>

        <table v-if="!interventionsListIsEmpty" class="table mt-5">
            <thead>
            <tr>
                <th scope="col"></th>
                <th scope="col">#</th>
                <th scope="col">Prénom</th>
                <th scope="col">Date</th>
                <th scope="col">Actions</th>
            </tr>
            </thead>
            <tbody class="interventions-list">
            <tr v-for="(intervention, index) in paginatedData" :key="index"
                :class="{completed: intervention.completed,}">
                <td><input type="checkbox" v-model="intervention.completed" class="toggle"></td>
                <td>{{ ++index }}</td>
                <td @dblclick="editName(intervention)">
                    <span v-if="!intervention.showEditName">{{intervention.name}}</span>
                    <input type="text" v-else v-model="intervention.name" @keyup.enter="doneEditName(intervention)">
                </td>
                <td>{{ intervention.date }}</td>
                <td>
                    <div class="d-flex">
                        <div class="mr-4" @click="showModal = true"><i class="fas fa-info"></i></div>
                        <div @click.prevent="deleteIntervention(intervention)"><i class="fas fa-trash-alt"></i></div>
                    </div>
                    <div class="modal is-active" v-if="showModal" @click="showModal = false">
                        <div class="modal-background"></div>
                        <div class="modal-content">
                            <div class="box">
                                <p>Demande d'intervention faite par : <strong style="font-size: 30px">
                                    {{intervention.name}}</strong></p>
                                <br>
                                <p style="font-size: 18px">" {{intervention.details}} "</p>
                                <br>
                                <p style="font-size: 12px">Cette demande d'intervention a été crée le <strong>{{intervention.date}}</strong>
                                </p>
                            </div>
                        </div>
                    </div>
                </td>
            </tr>
            </tbody>
            <div v-show="!interventionsListIsEmpty">

                <nav aria-label="Page navigation example">
                    <ul class="pagination justify-content-center">
                        <li class="page-item" :class="{disabled: pageNumber === 0}">
                            <a class="page-link" @click="prevPage">Previous</a>
                        </li>
                        <li class="page-item" :class="{disabled: pageNumber >= pageCount -1}">
                            <a class="page-link" @click="nextPage">Next</a>
                        </li>
                    </ul>
                </nav>
            </div>
        </table>

        <footer class="footer" v-show="!interventionsListIsEmpty">

            <span class="interventions-count"><strong>{{ remainingInterventions }}</strong> intervention(s) à traiter</span>
        </footer>
    </section>
</template>
<script>
    export default {
        data() {
            return {
                name: "",
                details: "",
                completed: false,
                newIntervention: "",
                showModal: false,
                showEditName: false,
                interventionsList: [],
                errors: [],
                pageNumber: 0
            }
        },
        props: {
            size: {
                type: Number,
                required: false,
                default: 3
            }
        },
        methods: {
            checkForm() {
                if (this.name && this.details) {
                    this.addIntervention();
                }
                this.errors = [];
                if (!this.name) {
                    this.errors.push('votre prénom, ');
                }
                if (!this.details) {
                    this.errors.push('les détails de votre demande d\'intervention');
                }
            },
            addIntervention() {
                this.interventionsList.push({
                    name: this.name,
                    details: this.details,
                    completed: false,
                    date: new Date().toLocaleString()
                });
            },
            deleteIntervention(intervention) {
                this.interventionsList = this.interventionsList.filter(i => i !== intervention)
            },
            editName(intervention) {
                const index = this.interventionsList.indexOf(intervention);
                intervention.showEditName = true;
                this.$set(this.interventionsList, index, intervention);
            },
            doneEditName(intervention) {
                const index = this.interventionsList.indexOf(intervention);
                intervention.showEditName = false;
                this.$set(this.interventionsList, index, intervention);
            },
            nextPage() {
                this.pageNumber++;
            }
            ,
            prevPage() {
                this.pageNumber--;
            }
        },
        computed: {
            interventionsListIsEmpty() {
                return this.interventionsList.length === 0;
            },
            remainingInterventions() {
                return this.interventionsList.filter(intervention => !intervention.completed).length
            },
            pageCount() {
                let l = this.interventionsList.length,
                    s = this.size;
                return Math.ceil(l / s);
            },
            paginatedData() {
                const start = this.pageNumber * this.size,
                    end = start + this.size;
                return this.interventionsList.slice(start, end)
            }
        }
    }
</script>
<style src="./interventions.css"></style>

