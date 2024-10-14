<template>
    <form class="flex flex-col gap-2" @submit.prevent="onSubmit">
        <input class="p-2 border rounded" v-model="formSubject" placeholder="کار جدید..." />
        <button class="p-2 bg-red-300 rounded" type="submit">افزودن کار جدید</button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            formSubject: ""
        };
    },
    methods: {
        onSubmit() {
            const newItem = {
                subject: this.formSubject,
                isCompleted: false,
                isEdit: false,
                isConfirmEdit: false,
                id: Date.now()
            };
            
            this.$emit('add-item', newItem);
            this.formSubject = "";
        },
    },
};
</script>
