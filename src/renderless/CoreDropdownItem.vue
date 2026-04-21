<script>
export default {
    name: 'CoreDropdownItem',

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

    render() {
        return this.$slots.default?.({
            current: this.current,
            select: this.select,
        });
    },
};
</script>
