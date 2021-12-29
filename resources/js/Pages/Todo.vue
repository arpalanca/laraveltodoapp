<template>
    <app-layout title="To Dos">
        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <div class="flex flex-col p-6">
                        <div class="text-center">
                            <p class="mt-1 text-4xl font-extrabold text-gray-900 sm:tracking-tight">
                                My To Do List
                            </p>

                            <div class="px-4 py-5 sm:p-6">
                                <form class="mt-5 flex items-center justify-center" @submit.prevent="submit">
                                    <div class="w-full sm:max-w-xs">
                                        <jet-label for="title" class="sr-only" value="Title" />
                                        <jet-input id="title" type="text" v-model="form.title" class="block w-full py-1" />
                                    </div>
                                    <jet-button class="ml-4" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                                        Add
                                    </jet-button>
                                </form>
                            </div>

                            <div class="rounded-md bg-red-50 p-4" v-if="$page.props.errors.title">
                                <div class="flex">
                                    <div class="flex-shrink-0">
                                        <XCircleIcon class="h-5 w-5 text-red-400" aria-hidden="true" />
                                    </div>
                                    <div class="ml-3">
                                        <h3 class="text-sm font-medium text-red-800">
                                            {{ $page.props.errors.title }}
                                        </h3>
                                    </div>
                                </div>
                            </div>

                            <div class="rounded-md bg-green-50 p-4" v-if="$page.props.jetstream.flash.message">
                                <div class="flex">
                                    <div class="flex-shrink-0">
                                        <CheckCircleIcon class="h-5 w-5 text-green-400" aria-hidden="true" />
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-sm font-medium text-green-800">
                                            {{ $page.props.jetstream.flash.message }}
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <div class="my-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
                            <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
                                <template v-if="!$page.props.data.length">
                                    <div class="text-center">
                                        <svg class="mx-auto h-12 w-12 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 48 48" aria-hidden="true">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M34 40h10v-4a6 6 0 00-10.712-3.714M34 40H14m20 0v-4a9.971 9.971 0 00-.712-3.714M14 40H4v-4a6 6 0 0110.713-3.714M14 40v-4c0-1.313.253-2.566.713-3.714m0 0A10.003 10.003 0 0124 26c4.21 0 7.813 2.602 9.288 6.286M30 14a6 6 0 11-12 0 6 6 0 0112 0zm12 6a4 4 0 11-8 0 4 4 0 018 0zm-28 0a4 4 0 11-8 0 4 4 0 018 0z" />
                                        </svg>
                                        <h2 class="mt-2 text-lg font-medium text-gray-900">Add to do</h2>
                                        <p class="mt-1 text-sm text-gray-500">
                                            You haven’t added any to dos yet. You can manage your own to dos in this workspace.
                                        </p>
                                    </div>
                                </template>
                                <template v-else>
                                    <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
                                        <table class="min-w-full divide-y divide-gray-200">
                                            <thead class="bg-gray-50">
                                                <tr>
                                                    <th scope="col" class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                        Title
                                                    </th>
                                                    <th scope="col" class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                        Status
                                                    </th>
                                                    <th scope="col" class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                        Action
                                                    </th>
                                                </tr>
                                            </thead>
                                            <tbody class="bg-white divide-y divide-gray-200">
                                                <tr v-for="todo in $page.props.data" :key="todo.id">
                                                    <td class="px-6 py-4 whitespace-nowrap">
                                                        <p class="text-sm text-gray-900" :class="{ 'line-through' : todo.completed }">
                                                            {{ todo.title }}
                                                        </p>
                                                    </td>
                                                    <td class="px-6 py-4 whitespace-nowrap text-center">
                                                        <template v-if="todo.completed">
                                                            <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-yellow-100 text-yellow-800">
                                                                Done
                                                            </span>
                                                        </template>
                                                        <template v-else>
                                                            <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">
                                                                Active
                                                            </span>
                                                        </template>
                                                    </td>
                                                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-center">
                                                        <Switch v-model="todo.completed" :class="[todo.completed ? 'bg-indigo-600' : 'bg-gray-200', 'relative inline-flex flex-shrink-0 h-6 w-11 border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500']" @click="updateTodo(todo)">
                                                            <span class="sr-only">Use setting</span>
                                                            <span :class="[todo.completed ? 'translate-x-5' : 'translate-x-0', 'pointer-events-none relative inline-block h-5 w-5 rounded-full bg-white shadow transform ring-0 transition ease-in-out duration-200']">
                                                                <span :class="[todo.completed ? 'opacity-0 ease-out duration-100' : 'opacity-100 ease-in duration-200', 'absolute inset-0 h-full w-full flex items-center justify-center transition-opacity']" aria-hidden="true">
                                                                    <svg class="h-3 w-3 text-gray-400" fill="none" viewBox="0 0 12 12">
                                                                        <path d="M4 8l2-2m0 0l2-2M6 6L4 4m2 2l2 2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                                                                    </svg>
                                                                </span>
                                                                <span :class="[todo.completed ? 'opacity-100 ease-in duration-200' : 'opacity-0 ease-out duration-100', 'absolute inset-0 h-full w-full flex items-center justify-center transition-opacity']" aria-hidden="true">
                                                                    <svg class="h-3 w-3 text-indigo-600" fill="currentColor" viewBox="0 0 12 12">
                                                                        <path d="M3.707 5.293a1 1 0 00-1.414 1.414l1.414-1.414zM5 8l-.707.707a1 1 0 001.414 0L5 8zm4.707-3.293a1 1 0 00-1.414-1.414l1.414 1.414zm-7.414 2l2 2 1.414-1.414-2-2-1.414 1.414zm3.414 2l4-4-1.414-1.414-4 4 1.414 1.414z" />
                                                                    </svg>
                                                                </span>
                                                            </span>
                                                        </Switch>

                                                        <jet-button class="ml-2 px-0 bg-transparent hover:bg-transparent" @click="deleteTodo(todo)">
                                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-red-600 hover:text-gray-700" viewBox="0 0 20 20" fill="currentColor">
                                                                <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                                                            </svg>
                                                        </jet-button>
                                                    </td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </template>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </app-layout>
</template>

<script>
    import {defineComponent} from 'vue'
    import AppLayout from '@/Layouts/AppLayout'
    import JetApplicationLogo from '@/Jetstream/ApplicationLogo.vue'
    import JetInput from '@/Jetstream/Input'
    import JetLabel from '@/Jetstream/Label'
    import JetButton from '@/Jetstream/Button'
    import { CheckCircleIcon, XCircleIcon } from '@heroicons/vue/solid'
    import { Switch } from '@headlessui/vue'

    export default defineComponent({
        name: "Todo",

        components: {
            AppLayout,
            JetApplicationLogo,
            JetInput,
            JetLabel,
            JetButton,
            CheckCircleIcon,
            Switch,
            XCircleIcon
        },

        props: ['data', 'errors'],

        data() {
            return {
                form: this.$inertia.form({
                    user_id: this.$page.props.user.id,
                    title: null,
                    completed: 0
                })
            }
        },

        methods: {
            submit() {
                this.form.post(this.route('todos.store'), {
                    onFinish: () => this.form.title = '',
                })
            },

            deleteTodo(todo) {
                todo._method = 'DELETE';
                this.$inertia.post('/todos/' + todo.id, todo);
            },

            updateTodo(todo) {
                todo._method = 'PUT';
                this.$inertia.post('/todos/' + todo.id, todo)
            }
        },
    })
</script>
