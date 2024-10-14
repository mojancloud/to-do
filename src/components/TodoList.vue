<template>
    <div class="max-w-5xl h-screen max-h-screen max-w-screen px-2 py-2 mx-auto flex flex-col justify-between gap-2">
        <div class="p-2 bg-gray-100 rounded h-screen overflow-auto">
            <div class="text-center py-2 bg-red-300 rounded">کارهای انجام نشده</div>
            <div v-if="items.filter(item => !item.isCompleted).length === 0" class="text-center text-gray-500 py-4">
                هیچ کار انجام نشده ای ندارید
            </div>
            <div v-for="x in items" :key="x.id">
                <div v-if="!x.isCompleted"
                    class="my-2 flex flex-row justify-between bg-gray-200 p-2 rounded border-r-4 border-red-400">
                    <TodoItem :subject="x.subject" :isEdit="x.isEdit" :isConfirmEdit="x.isConfirmEdit"
                        @update-subject="updateSubject(x, $event)" />
                    <div class="flex flex-row gap-1">
                        <div class="flex flex-row mx-1">
                            <div class="relative group">
                                <button class="w-8 h-8 bg-red-300 rounded-full mx-0.5" v-if="!x.isEdit"
                                    @click="completeItem(x)">✔️</button>
                                <Tooltip
                                    class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-1 w-max px-2 py-1 text-xs text-white bg-gray-700 rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"
                                    tooltip="علامت گذاری به عنوان انجام شده" />
                            </div>
                            <div class="relative group">
                                <button class="w-8 h-8 bg-red-300 rounded-full mx-0.5" v-if="!x.isEdit"
                                    @click="editItem(x)">✍️</button>
                                <Tooltip
                                    class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-1 w-max px-2 py-1 text-xs text-white bg-gray-700 rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"
                                    tooltip="ویرایش" />
                            </div>
                            <div class="relative group">
                                <button class="w-8 h-8 bg-red-300 rounded-full mx-0.5" v-if="!x.isEdit"
                                    @click="removeItem(x.id)">🗑️</button>
                                <Tooltip
                                    class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-1 w-max px-2 py-1 text-xs text-white bg-gray-700 rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"
                                    tooltip="حذف" />
                            </div>
                            <div class="relative group">
                                <button class="w-8 h-8 bg-red-300 rounded-full mx-0.5" v-if="x.isEdit"
                                    @click="cancelEdit(x)">❌</button>
                                <Tooltip
                                    class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-1 w-max px-2 py-1 text-xs text-white bg-gray-700 rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"
                                    tooltip="لغو ویرایش" />
                            </div>
                            <div class="relative group">
                                <button class="w-8 h-8 bg-red-300 rounded-full mx-0.5" v-if="x.isEdit"
                                    @click="confirmEdit(x)">✅</button>
                                <Tooltip
                                    class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-1 w-max px-2 py-1 text-xs text-white bg-gray-700 rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"
                                    tooltip="تایید ویرایش" />
                            </div>
                        </div>
                    </div>
                </div>
            </div>


            <div class="text-center py-2 bg-green-300 rounded">کارهای انجام شده</div>
            <div v-if="items.filter(item => item.isCompleted).length === 0" class="text-center text-gray-500 py-4">
                هیچ کار انجام شده ای ندارید
            </div>
            <div class="my-2" v-for="x in items" :key="x.id">
                <div v-if="x.isCompleted"
                    class="flex flex-row justify-between bg-gray-200 p-2 rounded border-r-4 border-green-400">
                    <TodoItem :subject="x.subject" :isEdit="x.isEdit" :isConfirmEdit="x.isConfirmEdit"
                        @update-subject="updateSubject(x, $event)" />
                    <div class="mx-1">
                        <div class="relative group">
                            <button class="w-8 h-8 bg-red-300 rounded-full mx-0.5" v-if="!x.isEdit"
                                @click="removeItem(x.id)">🗑️</button>
                            <Tooltip
                                class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-1 w-max px-2 py-1 text-xs text-white bg-gray-700 rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"
                                tooltip="حذف" />
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="p-2 bg-gray-100 rounded">
            <TodoForm @add-item="addItem($event)" />
        </div>
    </div>
</template>




<script>
import TodoForm from './TodoForm.vue';
import TodoItem from './TodoItem.vue';
import Tooltip from './Tooltip.vue';

export default {
    data() {
        return {
            items: this.loadItems()
        };
    },
    components: {
        TodoItem, TodoForm, Tooltip
    },
    methods: {
        loadItems() {
            const storedItems = JSON.parse(localStorage.getItem('todoItems'));
            return storedItems || [
                {
                    subject: "ساخت رابط کاربری ساده و کاربر پسند",
                    isCompleted: true,
                    isEdit: false,
                    isConfirmEdit: false,
                    id: 1,
                },
                {
                    subject: "امکان افزودن ایتم جدید",
                    isCompleted: true,
                    isEdit: false,
                    isConfirmEdit: false,
                    id: 2,
                }, 
                {
                    subject: "نمایش لیست آیتم ها",
                    isCompleted: true,
                    isEdit: false,
                    isConfirmEdit: false,
                    id: 3,
                },
                {
                    subject: "امکان ویرایش آیتم ها",
                    isCompleted: true,
                    isEdit: false,
                    isConfirmEdit: false,
                    id: 4,
                },
                {
                    subject: "امکان حذف آیتم ها",
                    isCompleted: true,
                    isEdit: false,
                    isConfirmEdit: false,
                    id: 5,
                },
                {
                    subject: "ذخیره محلی داده ها",
                    isCompleted: true,
                    isEdit: false,
                    isConfirmEdit: false,
                    id: 6,
                },
                {
                    subject: "انجام پروژه های بعدی",
                    isCompleted: false,
                    isEdit: false,
                    isConfirmEdit: false,
                    id: 7,
                },
            ];
        },
        saveItems() {
            localStorage.setItem('todoItems', JSON.stringify(this.items));
        },
        removeItem(id) {
            this.items = this.items.filter(item => item.id !== id);
            this.saveItems();
        },
        editItem(item) {
            item.isEdit = true;
        },
        cancelEdit(item) {
            item.isEdit = false;
        },
        confirmEdit(item) {
            item.isEdit = false;
            item.isConfirmEdit = true;
        },
        updateSubject(item, newSubject) {
            item.subject = newSubject;
            item.isConfirmEdit = false;
            this.saveItems();
        },
        addItem(newItem) {
            this.items.push(newItem);
            this.saveItems();
        },
        completeItem(x) {
            x.isCompleted = true;
            this.saveItems();
        },
    }
};
</script>
