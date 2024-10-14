<template>
    <div class="flex items-center">
        <p v-if="!isEdit">{{ subject }}</p>
        <input class="p-1 rounded" v-else v-model="newSubject" required />
    </div>
</template>

<script>
export default {
    props: {
        subject: {
            type: String,
        },
        isEdit: {
            type: Boolean,
        },
        isConfirmEdit: {
            type: Boolean
        }
    },
    data() {
        return {
            newSubject: this.subject
        }
    },
    watch: {
        isEdit(newValue) {
            if (newValue) {
                this.newSubject = this.subject;
            }
        },
        isConfirmEdit(newValue) {
            if (newValue) {
                this.$emit('update-subject', this.newSubject);
            }
        }
    }
}
</script>
