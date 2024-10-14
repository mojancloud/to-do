<template>
    <span
        class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-1 w-max px-2 py-1 text-xs text-white bg-gray-700 rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none">
        {{ tooltip }}
    </span>
</template>

<script>
export default {
    props: {
        tooltip: {
            type: String,
            default: "default"
        },
    },
};
</script>
