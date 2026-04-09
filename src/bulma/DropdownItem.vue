<template>
    <a class="dropdown-item"
        :class="[{
            'is-active': selected,
            'is-current': current && !selected }
        ]"
        @click="select"
        @mouseenter="makeCurrent(this)">
        <slot name="default"
            :current="current"/>
    </a>
</template>

<script>
export default {
    name: 'DropdownItem',

    inject: ['attemptHide', 'canSelect', 'deregister', 'makeCurrent', 'register'],

    props: {
        selected: {
            type: Boolean,
            default: false,
        },
    },

    emits: ['select'],

    data: () => ({
        current: false,
        ref: 'item',
    }),

    created() {
        this.register(this);
    },

    beforeUnmount() {
        this.deregister(this);
    },

    mounted() {
        this.$el.__item__ = this;
    },

    methods: {
        select() {
            if (this.canSelect()) {
                this.$emit('select');
                this.attemptHide();
            }
        },
    },
};
</script>

<style lang="scss">
    .dropdown-item.is-current,
    .dropdown-item.is-active,
    .dropdown-item:hover {
        background-color: var(
            --bulma-dropdown-item-hover-background-color,
            var(--bulma-scheme-main-ter)
        );
        color: var(--bulma-text-strong);
    }
</style>
