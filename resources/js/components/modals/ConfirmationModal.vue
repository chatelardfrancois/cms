<template>
    <modal name="confirmation-modal" @opened="$emit('opened')">
        <form class="confirmation-modal flex flex-col h-full" @submit.prevent="submit">
            <header v-if="title" class="text-lg font-semibold px-5 py-3 bg-gray-200 dark:bg-dark-550 rounded-t-lg flex items-center justify-between border-b dark:border-dark-900">
                {{ __(title) }}
            </header>
            <div class="flex-1 px-5 py-6 text-gray dark:text-dark-150">
                <slot name="body">
                    <p v-if="bodyText" v-text="bodyText" />
                    <slot v-else>
                        <p>{{ __('Are you sure?') }}</p>
                    </slot>
                </slot>
            </div>
            <div class="px-5 py-3 bg-gray-200 dark:bg-dark-550 rounded-b-lg border-t dark:border-dark-900 flex items-center justify-end text-sm">
                <button type="button" class="btn-flat" @click.prevent="$emit('cancel')" v-text="__(cancelText)" v-if="cancellable" />
                <button class="rtl:mr-4 ltr:ml-4" :class="buttonClass" :disabled="disabled" v-text="__(buttonText)" />
            </div>
        </form>
    </modal>
</template>

<script>
export default {
    props: {
        title: {
            type: String
        },
        bodyText: {
            type: String
        },
        buttonText: {
            type: String,
            default: 'Confirm'
        },
        cancellable: {
            type: Boolean,
            default: true
        },
        cancelText: {
            type: String,
            default: 'Cancel'
        },
        danger: {
            type: Boolean,
            default: false
        },
        disabled: {
            type: Boolean,
            default: false,
        }
    },

    data() {
        return {
            escBinding: null,
            enterBinding: null,
        }
    },

    computed: {
        buttonClass() {
            return this.danger ? 'btn-danger' : 'btn-primary';
        }
    },

    methods: {
        dismiss() {
            this.$emit('cancel')
        },
        submit() {
            this.$emit('confirm')
        }
    },

    created() {
        this.escBinding = this.$keys.bind('esc', this.dismiss)
    },

     beforeDestroy() {
        this.escBinding.destroy()
    },
}
</script>
