<template>
    <dash-layout title="My Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                My Dashboard
            </h2>
        </template>
        <div class="h-full">
           <!-- add content here -->
        </div>
    </dash-layout>
</template>

<script>
    import { defineComponent } from 'vue'
    import DashLayout from '@/Layouts/DashLayout.vue'
    import JamStatCard from '@/components/StatCard.vue'

    export default defineComponent({
        components: {
            DashLayout,
            JamStatCard
        },
    })
</script>
