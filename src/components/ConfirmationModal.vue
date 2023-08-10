<template>
    <div class="modal-backdrop" @click="$emit('close')">
        <div class="modal" role="dialog" aria-labelledby="modal" aria-describedby="modal" @click.stop>
            <header class="modal-header">
                <h3>Are you sure you want to delete "{{ name }}"?</h3>
            </header>

            <section class="modal-body">
                <button class="confirm-button" @click="$emit('confirm', id)" aria-label="Confirm">Yes</button>
                <button class="deny-button" @click="$emit('close')" aria-label="Deny">No</button>
            </section>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ConfirmationModal',
    data: function () {
        return {
            id: '',
            name: '',
        };
    },
    methods: {
        setData(data) {
            const { id, name } = data;
            this.id = id;
            this.name = name;
        },
    },
};
</script>

<style scoped>
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
    width: 320px;
}

.modal-body {
    margin-bottom: 10px;
}

button {
    border: none;
    padding: 10px 20px;
}

.confirm-button {
    background-color: rgb(235, 235, 235);
    margin-right: 5px;
}

.confirm-button:hover {
    background-color: rgba(0, 255, 0, 0.5);
}

.deny-button {
    background-color: rgba(255, 0, 0, .75);
}

.deny-button:hover {
    background-color: rgba(255, 0, 0, 1);
}
</style>
