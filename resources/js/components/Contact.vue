<template>
    <div>
        <!-- Button trigger modal -->
        <button @click="update = false; openModal()" type="button" class="btn btn-primary">
            Novo Contato
        </button>

        <!-- Modal -->
        <div class="modal fade" :class="{show:modal}">
            <div class="modal-dialog modal-lg modal-dialog-scrollable">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">{{titleModal}}</h5>
                        <button @click="closeModal()" type="button" class="close" data-dismiss="modal"
                                aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <label for="first_name">Nome</label>
                        <input v-model="contact.first_name" type="text" class="form-control" id="first_name">
                        <label for="last_name">Sobrenome</label>
                        <input v-model="contact.last_name" type="text" class="form-control" id="last_name">
                        <label for="email">E-mail</label>
                        <input v-model="contact.email" type="email" class="form-control" id="email">
                        <label for="phone">Telefone</label>
                        <input v-model="contact.phone" type="text" class="form-control" id="phone">
                        <label for="address">Endereço</label>
                        <input v-model="contact.address" type="text" class="form-control" id="address">
                    </div>
                    <div class="modal-footer">
                        <button @click="closeModal()" type="button" class="btn btn-secondary" data-dismiss="modal">
                            Fechar
                        </button>
                        <button @click="save()" type="button" class="btn btn-success">Salvar Alterações</button>
                    </div>
                </div>
            </div>
        </div>
        <h1 class="text-center">Agenda de Contatos</h1>
        <table class="table table-hover table-striped">
            <thead>
            <tr>
                <th scope="col">#</th>
                <th scope="col">Nome</th>
                <th scope="col">Sobrenome</th>
                <th scope="col">E-mail</th>
                <th scope="col">Telefone</th>
                <th scope="col">Endereço</th>
                <th scope="col" colspan="2">Ação</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="contact in contacts" :key="contact.id">
                <th scope="row">{{contact.id}}</th>
                <td>{{contact.first_name}}</td>
                <td>{{contact.last_name}}</td>
                <td>{{contact.email}}</td>
                <td>{{contact.phone}}</td>
                <td>{{contact.address}}</td>
                <td>
                    <button @click="update=true; openModal(contact)" class="btn btn-warning">Editar</button>
                </td>
                <td>
                    <button @click="remove(contact.id)" class="btn btn-danger">Excluir</button>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                contact: {
                    first_name: '',
                    last_name: '',
                    email: '',
                    phone: '',
                    address: '',
                },
                id: 0,
                update: true,
                modal: 0,
                titleModal: '',
                contacts: []
            }
        },
        methods: {
            async list() {
                const res = await axios.get('contacts')
                this.contacts = res.data
            },
            async remove(id) {
                await axios.delete(`/contacts/${id}`)
                this.list()
            },
            async save() {
                if (this.update) {
                    const res = await axios.put('/contacts/' + this.id, this.contact)
                } else {
                    const res = await axios.post('/contacts', this.contact)
                }
                this.list()
                this.closeModal()
            },
            openModal(data = {}) {
                this.modal = 1
                if (this.update) {
                    this.titleModal = 'Alterar Contato'
                    this.id = data.id
                    this.contact = data
                } else {
                    this.id = 0
                    this.titleModal = 'Criar Contato'
                    this.contact = {}
                }
            },
            closeModal() {
                this.modal = 0
            }
        },
        created() {
            this.list()
        }
    }
</script>

<style>
    .show {
        display: list-item;
        opacity: 1;
        background: rgba(57, 65, 75, 0.85);
    }
</style>