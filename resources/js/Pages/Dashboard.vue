<template>
    <app-layout title="To Dos">
        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <todo />
                </div>
            </div>
        </div>
    </app-layout>
</template>

<script>
    import { defineComponent } from 'vue'
    import AppLayout from '@/Layouts/AppLayout'
    import Todo from '@/Pages/Todo'

    export default defineComponent({
        components: {
            AppLayout,
            Todo,
        },
    })
</script>
