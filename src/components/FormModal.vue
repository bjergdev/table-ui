<template>
    <div class="modal-backdrop" @click="close">
        <div class="modal" role="dialog" aria-labelledby="modal" aria-describedby="modal" @click.stop>
            <header class="modal-header">
                <button class="btn-close" aria-label="Close modal" @click="close">
                    X
                </button>

                <h2>{{ headerText }} book</h2>
                <p>{{ descriptionText }} the book details</p>
            </header>

            <section class="modal-body">
                <ValidationObserver ref="observer" v-slot="{ handleSubmit }">
                    <form @submit.prevent="handleSubmit(submitForm)">
                        <ValidationProvider class="validation-row" name="name" rules="required" v-slot="{ errors }">
                            <div class="input-row">
                                <label for="name">Name: </label>

                                <input v-model="name" name="name" id="name" />
                            </div>
                            <span class="error">{{ errors[0] }}</span>
                        </ValidationProvider>
                        <ValidationProvider class="validation-row" name="author" rules="required" v-slot="{ errors }">
                            <div class="input-row">
                                <label for="author">Author: </label>
                                <input v-model="author" name="author" id="author" />
                            </div>
                            <span class="error">{{ errors[0] }}</span>
                        </ValidationProvider>
                        <ValidationProvider class="validation-row" name="quantity" rules="required|integer" v-slot="{ errors }">
                            <div class="input-row">
                                <label for="quantity">Quantity: </label>
                                <input v-model="quantity" name="quantity" id="quantity" />
                            </div>
                            <span class="error">{{ errors[0] }}</span>
                        </ValidationProvider>
                        <button type="submit" class="btn-green" aria-label="Submit">{{ submitButtonText }}</button>
                    </form>
                </ValidationObserver>
            </section>
        </div>
    </div>
</template>

<script>
import { ValidationObserver, ValidationProvider } from 'vee-validate/dist/vee-validate.full.esm';
import { MODAL_MODE } from '../consts.js';

export default {
    name: 'FormModal',
    components: {
        ValidationObserver,
        ValidationProvider,
    },
    data: function () {
        return {
            mode: MODAL_MODE.ADD,
            id: '',
            name: '',
            author: '',
            quantity: '',
        };
    },
    computed: {
        headerText() {
            return this.mode === MODAL_MODE.EDIT ? 'Edit' : 'Add';
        },
        descriptionText() {
            return this.mode === MODAL_MODE.EDIT ? 'Change' : 'Fill in';
        },
        submitButtonText() {
            return this.mode === MODAL_MODE.EDIT ? 'Change' : 'Add';
        },
    },
    methods: {
        submitForm() {
            this.$refs.observer.validate().then(valid => {
                if (!valid) return;
                const payload = {
                    id: this.id,
                    name: this.name,
                    author: this.author,
                    quantity: this.quantity,
                };
                this.$emit(this.mode, payload);
            });
        },
        close() {
            this.clearForm();
            this.resetErrors();
            this.$emit('close');
        },
        clearForm() {
            this.mode = MODAL_MODE.ADD;
            this.id = '';
            this.name = '';
            this.author = '';
            this.quantity = '';
        },
        resetErrors() {
            this.$refs.observer.reset();
        },
        setData(data) {
            const { id, name, author, quantity } = data;
            this.mode = MODAL_MODE.EDIT;
            this.id = id;
            this.name = name;
            this.author = author;
            this.quantity = quantity;
        },
    },
};
</script>

<style scoped>
input {
    padding: 5px;
}

.modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    width: 300px;
}

.modal-header {
    padding: 15px 20px;
    display: flex;
    position: relative;
    border-bottom: 1px solid #eeeeee;
    justify-content: space-between;
    flex-direction: column;
}

.modal-header h1,
.modal-header h2,
.modal-header h3,
.modal-header h4,
.modal-header h5,
.modal-header h6 {
    color: rgba(154,205,50, 1);
    margin: 0;
}

.modal-header p {
    margin-bottom: 0;
}

.modal-body {
    position: relative;
    padding: 20px 15px;
}

.modal-body .input-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 10px 0;
}

.modal-body .input-row:not(:first-child) {
    margin-top: 16px;
}

.modal-body .input-row .validation-row {
    display: inline-flex;
    flex-direction: column;
}

.btn-close {
    position: absolute;
    top: 0;
    right: 0;
    border: none;
    font-size: 20px;
    padding: 10px;
    cursor: pointer;
    font-weight: bold;
    color: rgba(154,205,50, .75);
    background: transparent;
}

.btn-close:hover {
    color: rgba(154,205,50, 1);
}

.btn-green {
    display: block;
    margin: auto;
    color: black;
    background-color: rgba(154,205,50, .75);
    border: 1px solid rgba(154,205,50, .75);
    border-radius: 2px;
    padding: 5px 15px;
    margin-top: 25px;
}

.btn-green:hover {
    background-color: rgba(154,205,50, 1);
    border-color: rgba(154,205,50, 1);
}

.error {
    color: red;
}
</style>
