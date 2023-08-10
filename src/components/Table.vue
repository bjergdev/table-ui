<template>
    <table>
        <tr>
            <th>ID</th>
            <th>Book name</th>
            <th>Author name</th>
            <th>Book quantity</th>
            <th></th>
        </tr>
        <tr
            v-for="book in books"
            :key="book.id"
        >
            <td>{{ book.id }}</td>
            <td>{{ book.name }}</td>
            <td>{{ book.author }}</td>
            <td>{{ book.quantity }}</td>
            <td>
                <div class="action-buttons">
                    <button class="edit-button" @click="$emit('edit', book)">Edit <mdicon name="book-edit-outline" /></button>
                    <button class="delete-button" @click="$emit('delete', book.id)">Delete <mdicon name="delete" /></button>
                </div>
            </td>
        </tr>
    </table>
</template>

<script>
export default {
    name: 'TableComponent',
    props: {
        books: Array,
    },
}
</script>

<style scoped>
table {
    border-collapse: collapse;
    border-radius: 50%;
}

th {
    background-color: rgb(54, 48, 74);
    color: white;
}

tr {
    background-color: white;
}

tr:nth-child(odd):not(:first-child) {
    background-color: rgb(235, 235, 235);
}

table tr:first-child th:first-child {
    border-top-left-radius: 8px;
}

table tr:first-child th:last-child {
    border-top-right-radius: 8px;
}

table tr:last-child td:first-child {
    border-bottom-left-radius: 8px;
}

table tr:last-child td:last-child {
    border-bottom-right-radius: 8px;
}

th, td {
    padding: 12px 20px;
}

button {
    border: none;
    padding: 5px;
    font-weight: bold;
    min-width: 78px;
}

.action-buttons {
    display: flex;
    gap: 5px;
}

.edit-button {
    background-color: rgba(240,230,140, .5);
}

.edit-button:hover {
    background-color: rgba(240,230,140, 1)
}

.delete-button {
    background-color: rgba(220,20,60, .5);
}

.delete-button:hover {
    background-color: rgba(220,20,60, 1);
}
</style>
