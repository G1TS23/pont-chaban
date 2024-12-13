<template>
    <div class="header">
        <div>Fermetures du Pont Chaban-Delmas</div>
        <TestHour />
    </div>
</template>

<script setup>
import TestHour from "@/components/TestHour.vue"
</script>

<style scoped>

.header{
    display: flex;
    width: 100%;
    max-width: 500px;
    align-items: center;
    justify-content: space-between;
    font-size: 1rem;
    font-weight: 600;
    position: sticky;
    top: 0;
}

</style>