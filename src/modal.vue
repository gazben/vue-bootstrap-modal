<template>
    <div v-show="show" :transition="transition">
        <div class="modal" @click.self="clickMask">
            <div class="modal-dialog" :class="modalClass">
                <div class="modal-content">
                    <!--Header-->
                    <div class="modal-header">
                        <slot name="header">
                            <button type="button" class="close" @click="cancel"></button>
                            <h4 class="modal-title">
                                {{ title }}
                            </h4>
                        </slot>
                    </div>
                    <!--Container-->
                    <div class="modal-body">
                        {{ body }}
                    </div>
                    <!--Footer-->
                    <div class="modal-footer">
                        <slot name="footer">
                            <button type="button" :class="cancelClass" @click="cancel">{{ cancelText }}</button>
                            <button type="button" :class="okClass" @click="ok">{{ okText }}</button>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
        <div class="modal-backdrop in"></div>
    </div>
</template>

<script>
    export default {
        props: {
            show: {
                type: Boolean,
                twoWay: true,
                default: false
            },
            title: {
                type: String,
                default: 'Modal'
            },
            body: {
                type: String
            },
            small: {
                type: Boolean,
                default: false
            },
            large: {
                type: Boolean,
                default: false
            },
            full: {
                type: Boolean,
                default: false
            },
            force: {
                type: Boolean,
                default: false
            },
            transition: {
                type: String,
                default: 'modal'
            },
            okText: {
                type: String,
                default: 'Ok'
            },
            cancelText: {
                type: String,
                default: 'Cancel'
            },
            okClass: {
                type: String,
                default: 'btn btn-primary'
            },
            cancelClass: {
                type: String,
                default: 'btn btn-warning'
            },
            closeWhenOK: {
                type: Boolean,
                default: false
            }
        },
        data () {
            return {
                duration: null
            }
        },
        computed: {
            modalClass () {
                return {
                    'modal-lg': this.large,
                    'modal-sm': this.small,
                    'modal-full': this.full
                }
            }
        },
        created () {
            if (this.show) {
                document.body.className += ' modal-open'
            }
        },
        destroyed () {
            document.body.className = document.body.className.replace(' modal-open', '')
        },
        watch: {
            show (value) {
                if (value) {
                    document.body.className += ' modal-open'
                } else {
                    if (!this.duration) {
                        this.duration = window.getComputedStyle(this.$el)['transition-duration'].replace('s', '') * 1000
                    }

                    window.setTimeout(() => {
                        document.body.className = document.body.className.replace(' modal-open', '')
                    }, this.duration || 0)
                }
            }
        },
        methods: {
            ok () {
                this.$emit('ok')
                if (this.closeWhenOK) {
                    this.show = false
                }
            },
            cancel () {
                this.$emit('cancel')
                this.show = false
            },
            clickMask () {
                if (!this.force) {
                    this.cancel()
                }
            }
        }
    }
</script>

<style scoped>
    .modal {
        display: block
    }
    .modal-transition {
        transition: all .6s ease
    }
    .modal-leave {
        border-radius: 1px !important
    }
    .modal-transition .modal-dialog, .modal-transition .modal-backdrop {
        transition: all .5s ease
    }
    .modal-enter .modal-dialog, .modal-leave .modal-dialog {
        opacity: 0;
        transform: translateY(-30%)
    }
    .modal-enter .modal-backdrop, .modal-leave .modal-backdrop {
        opacity: 0
    }
</style>
