<template>
    <div id="app">
        <ConfirmationModal
            v-show="isConfirmationModalVisible"
            ref="confirmationModal"
            @confirm="handleConfirm"
            @close="closeConfirmationModal"
        />
        <FormModal
            v-show="isFormModalVisible"
            ref="formModal"
            @add="handleAddAction"
            @edit="handleEditAction"
            @close="closeFormModal"
        />
        <button
            class="add-button"
            @click="showFormModal"
        >
            <span><mdicon name="plus" />Add a new book</span>
        </button>
        <div class="table-wrapper">
            <Table
                :books="books"
                @delete="showConfirmModal"
                @edit="editBookModal"
            />
        </div>
    </div>
</template>

<script>
import FormModal from './components/FormModal.vue';
import ConfirmationModal from './components/ConfirmationModal.vue';
import Table from './components/Table.vue';

export default {
    name: 'App',
    components: {
        FormModal,
        ConfirmationModal,
        Table,
    },
    data: function () {
        return {
            isConfirmationModalVisible: false,
            isFormModalVisible: false,
            books: [
                {
                    id: 1,
                    name: 'The Lord of the Rings',
                    author: 'J.R.R. Tolkien',
                    quantity: 120,
                },
                {
                    id: 2,
                    name: 'Pinocchio',
                    author: 'Carlo Collodi',
                    quantity: 311,
                },
                {
                    id: 3,
                    name: 'Alice\'s Adventures in Wonderland',
                    author: 'Lewis Carroll',
                    quantity: 47,
                },
                {
                    id: 4,
                    name: 'Harry Potter and the Sorcerer\'s Stone',
                    author: 'J.K. Rowling',
                    quantity: 214,
                },
                {
                    id: 5,
                    name: 'The Lion King',
                    author: 'Justine Korman',
                    quantity: 98,
                },
                {
                    id: 6,
                    name: 'Iron Flame',
                    author: 'Rebecca Yarros',
                    quantity: 103,
                },
            ],
        };
    },
    methods: {
        addBook(data) {
            const { name, author, quantity } = data;
            const lastBookID = this.books[this.books.length - 1]?.id || 0;
            this.books.push({
                id: lastBookID + 1,
                name,
                author,
                quantity,
            });
        },
        findBookIndex(id) {
            const searchedBook = this.books.find(book => book.id === id);
            return this.books.indexOf(searchedBook);
        },
        showConfirmModal(id) {
            const book = this.books.find(book => book.id === id);
            this.$refs.confirmationModal.setData({ id: book.id, name: book.name });
            this.isConfirmationModalVisible = true;
        },
        deleteBook(id) {
            const bookIndex = this.findBookIndex(id);
            if (bookIndex === -1) return;
            this.books.splice(bookIndex, 1);
        },
        editBookModal(data) {
            this.$refs.formModal.setData(data);
            this.showFormModal();
        },
        editExistingBook(data) {
            const bookIndex = this.findBookIndex(data.id);
            if (bookIndex === undefined) return;
            this.books.splice(bookIndex, 1, data);
        },
        handleAddAction(data) {
            this.addBook(data);
            this.$refs.formModal.close();
        },
        handleEditAction(data) {
            this.editExistingBook(data);
            this.$refs.formModal.close();
        },
        handleConfirm(id) {
            this.deleteBook(id);
            this.closeConfirmationModal();
        },
        closeConfirmationModal() {
            this.isConfirmationModalVisible = false
        },
        closeFormModal() {
            this.isFormModalVisible = false;
        },
        showFormModal() {
            this.isFormModalVisible = true;
        },
    },
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    display: flex;
    flex-direction: column;
}

.add-button {
    align-self: flex-start;
    border: none;
    padding: 10px 15px 10px 10px;
    margin-bottom: 10px;
    background-color: rgba(154,205,50, .75);
    font-weight: bold;
}

.add-button:hover {
    background-color: rgba(154,205,50, 1);
}

.add-button span {
    margin-right: 10px;
}

.table-wrapper {
    overflow-x: auto;
}

.table-wrapper table {
    min-width: 700px;
}

button {
    cursor: pointer;
}
</style>
