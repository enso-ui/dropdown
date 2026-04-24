<template>
    <div class="dropdown is-active vue-dropdown"
        :class="[{'is-up': opensUp || isUp}, $attrs.class]">
        <core-dropdown v-bind="$attrs"
            ref="dropdown">
            <template #default="{
                keydown, hide, open, selection, show, triggerEvents,
            }">
                <div class="dropdown-trigger"
                    @keydown="keydown">
                    <slot name="trigger"
                        :hide="hide"
                        :open="open"
                        :opens-up="opensUp || isUp"
                        :show="show"
                        :selection="selection"
                        :trigger-events="triggerEvents">
                        <button class="button input"
                            :class="{ 'is-open': open }"
                            type="button"
                            v-on="triggerEvents">
                            <slot name="label"/>
                            <dropdown-indicator
                                :open="open"
                                :opens-up="opensUp || isUp"
                                v-if="!$attrs.disabled && !$attrs.readonly"/>
                        </button>
                    </slot>
                </div>
                <fade>
                    <div class="dropdown-menu"
                        @keydown="keydown"
                        v-click-outside="hide"
                        v-fits-below="fitsBelow"
                        v-if="open">
                        <div class="dropdown-content">
                            <slot name="controls"
                                :keydown="keydown"
                                :hide="hide"/>
                            <div class="items no-scrollbars">
                                <slot name="items"/>
                            </div>
                        </div>
                    </div>
                </fade>
            </template>
        </core-dropdown>
    </div>
</template>

<script>
import { clickOutside, fitsBelow } from '@enso-ui/directives';
import { Fade } from '@enso-ui/transitions';
import DropdownIndicator from '@enso-ui/dropdown-indicator';
import CoreDropdown from '../renderless/CoreDropdown.vue';

export default {
    name: 'Dropdown',

    directives: { clickOutside, fitsBelow },

    props: {
        opensUp: {
            type: Boolean,
            default: false,
        },
    },

    components: {
        CoreDropdown, Fade, DropdownIndicator,
    },

    inheritAttrs: false,

    data: () => ({
        isUp: false,
    }),

    methods: {
        hide() {
            this.$refs.dropdown.hide();
        },
        fitsBelow(state) {
            this.isUp = !state;
        },
        show() {
            this.$refs.dropdown.show();
        },
    },
};
</script>

<style lang="scss">
    .dropdown.vue-dropdown {
        .dropdown-trigger {
            .button.input {
                font-family: var(--bulma-body-family);
                min-width: var(--bulma-control-height);
                justify-content: flex-start;
                transition: border-color .18s ease, box-shadow .18s ease, background-color .18s ease;

                &:hover {
                    border-color: var(--bulma-border-hover);
                }

                &:focus,
                &:focus-visible,
                &.is-open {
                    border-color: var(--bulma-border-active);
                    box-shadow: none;
                }

                .dropdown-indicator {
                    position: absolute;
                    [dir='ltr'] & {
                        right: 0.5rem;
                    }
                    [dir='rtl'] & {
                        left: 0.5rem;
                    }
                }
            }
        }

        .dropdown-menu {
            min-width: unset;
        }

        .dropdown-content {
            width: fit-content;
            .items {
                max-height: 12rem;
                overflow: auto;
                width: inherit;
            }
        }
    }
</style>
