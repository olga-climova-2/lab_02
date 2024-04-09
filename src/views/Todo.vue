<script setup>
import { ref, computed, reactive } from 'vue';
const items = ref([
    {
        name: 'Список покупок',
        noteItems: ['Молоко', 'Кукуруза', 'Капуста'],
        checkedItems: [false, false, false]
    },
    {
        name: 'Список дел',
        noteItems: ['Прочитать теорию', 'Приступить к практике', 'Прочитать еще теорию', "Закончить заказ", "Сдать заказ"],
        checkedItems: [false, false, false, false, false]
    }]);

const selectedNote = ref(-1);

const selectedItem = ref(-1);

const editingNote = ref(-1);

const editingItem = ref(-1);

const itemUnderEditingNewText = ref("");

const notesListIsEmpty = computed(() => items.value.length === 0);

const noteIsNotSelected = computed(() => selectedNote.value === -1);

const selectNote = (index) => {
    selectedItem.value = -1;
    selectedNote.value = index;
}

const selectItem = (index) => {
    selectedItem.value = index;
}

const addNote = () => {
    items.value.push({
        name: 'Новая заметка',
        noteItems: [],
        checkedItems: []
    });
}

const addItem = () => {
    items.value[selectedNote.value].noteItems.push('Новый пункт');
    items.value[selectedNote.value].checkedItems.push(false);
}

const removeNote = (index) => {
    items.value.splice(index, 1);
}

const removeItem = (index) => {
    selectedItem.value = -1;
    items.value[selectedNote.value].noteItems.splice(index, 1);
}

const editNote = (index) => {
    editingNote.value = index;
}

const doneEditing = () => {
    editingNote.value = -1;
}

const editItem = (index) => {
    editingItem.value = index;
    itemUnderEditingNewText.value = items.value[selectedNote.value].noteItems[editingItem.value];
}

const doneEditingItem = () => {
    items.value[selectedNote.value].noteItems[editingItem.value] = itemUnderEditingNewText.value;
    editingItem.value = -1;
}

const isUnderEdit = (index) => {
    return index === editingNote.value;
}

const itemIsUnderEdit = (index) => {
    return index === editingItem.value;
}

const isChecked = (index) => {
    return items.value[selectedNote.value].checkedItems[index];
}

const check = (index) => {
    items.value[selectedNote.value].checkedItems[index] = !items.value[selectedNote.value].checkedItems[index];
}
</script>
<template>
    <div class="container">
        <div class="row align-items-start">
            <div class="col">
                <h1 class="title">Мои заметки</h1>
                <div v-if="notesListIsEmpty">Нет заметок...</div>
                <ul type="none" v-for="(item, index) in items">
                    <li class="row list-item" @click="selectNote(index)" :class="{ selected: index === selectedNote }">
                        <div v-if="isUnderEdit(index)">
                            <input v-model="item.name">
                            <button @click="removeNote(index)" type="button"
                                class="btn mx-1 btn-labeled btn-danger float-end">
                                <span class="btn-label"><i class="bi bi-trash"></i></span>
                            </button>
                            <button @click="doneEditing(index)" type="button"
                                class="btn mx-1 btn-labeled btn-success float-end">
                                <span class="btn-label"><i class="bi bi-check-lg"></i></span>
                            </button>
                        </div>
                        <div v-else>
                            {{ item.name }}
                            <button @click="removeNote(index)" type="button"
                                class="btn mx-1 btn-labeled btn-danger float-end">
                                <span class="btn-label"><i class="bi bi-trash"></i></span>
                            </button>
                            <button @click="editNote(index)" type="button"
                                class="btn mx-1 btn-labeled btn-warning float-end">
                                <span class="btn-label"><i class="bi bi-pencil"></i></span>
                            </button>
                        </div>
                    </li>
                </ul>
                <div class="d-grid gap-2">
                    <button @click="addNote" class="btn btn-primary" type="button">+ Добавить заметку</button>
                </div>
            </div>

            <div class="col">
                <h1 class="title">Содержание заметки</h1>
                <div v-if="noteIsNotSelected">Заметка не выбрана...</div>
                <ul v-else type="none" v-for="(item, index) in items[selectedNote].noteItems">
                    <li class="row list-item" @click="selectItem(index)" :class="{ selected: index === selectedItem }">
                        <div v-if="itemIsUnderEdit(index)">
                            <input v-model="itemUnderEditingNewText">
                            <button @click="removeItem(index)" type="button"
                                class="btn mx-1 btn-labeled btn-danger float-end">
                                <span class="btn-label"><i class="bi bi-trash"></i></span>
                            </button>
                            <button @click="doneEditingItem(index)" type="button"
                                class="btn mx-1 btn-labeled btn-success float-end">
                                <span class="btn-label"><i class="bi bi-check-lg"></i></span>
                            </button>
                        </div>
                        <div v-else>
                            <input v-if="isChecked(index)" @click="check(index)" class="form-check-input" type="checkbox" value="" checked>
                            <input v-else @click="check(index)" class="form-check-input" type="checkbox" value="">
                            {{ item }}
                            <button @click="removeItem(index)" type="button"
                                class="btn mx-1 btn-labeled btn-danger float-end">
                                <span class="btn-label"><i class="bi bi-trash"></i></span>
                            </button>
                            <button @click="editItem(index)" type="button"
                                class="btn mx-1 btn-labeled btn-warning float-end">
                                <span class="btn-label"><i class="bi bi-pencil"></i></span>
                            </button>
                        </div>
                    </li>
                </ul>
                <div class="d-grid gap-2">
                    <button @click="addItem" class="btn btn-primary" type="button">+ Добавить новый пункт</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
</script>
