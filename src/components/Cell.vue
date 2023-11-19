<template>
    <span :class="{selected}">{{ label }}</span>
</template>

<script setup lang="ts">
defineProps<{
  label: string;
  selected: boolean;
}>();
</script>

<style scoped>
    .selected {
        border: solid;
    }
    span {
        margin: .3em;
    }
</style>