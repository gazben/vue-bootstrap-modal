<script>
    /**
     * Bootstrap Style Modal Component for Vue
     * Depend on Bootstrap.css
     */

     export default {
        props: {
            show: {
                type: Boolean,
                default: false
            },
            title: {
                type: String,
                default: 'Modal'
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
            // 为true时无法通过点击遮罩层关闭modal
            force: {
                type: Boolean,
                default: false
            },
            // 自定义组件transition
            transition: {
                type: String,
                default: 'modal'
            },
            // 确认按钮text
            okText: {
                type: String,
                default: 'OK'
            },
            // 取消按钮text
            cancelText: {
                type: String,
                default: 'Cancel'
            },
            // 确认按钮className
            okClass: {
                type: String,
                default: 'btn blue'
            },
            // 取消按钮className
            cancelClass: {
                type: String,
                default: 'btn red btn-outline'
            },
            // 点击确定时关闭Modal
            // 默认为false，由父组件控制prop.show来关闭
            closeWhenOK: {
                type: Boolean,
                default: false
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
        watch: {
            show (value) {
                if (value) {
                    // 太长的modal，在显示时去掉body滚动条，防止出现双滚动条
                    let style = window.getComputedStyle(this.$els.dialog),
                        mt = +style['margin-top'].replace('px', ''),
                        mb = +style['margin-bottom'].replace('px', '');

                    if (this.$els.dialog.clientHeight + mt + mb + 10 > window.innerHeight) {
                        //document.body.className += ' modal-open-noscroll';
                        document.body.style.overflowY = 'hidden';
                    }
                } else {
                    // 在modal动画结束后再加上body滚动条
                    let duration = window.getComputedStyle(this.$el)['transition-duration'].replace('s', '') * 1000;

                    window.setTimeout(() => {
                        //document.body.className = document.body.className.replace('modal-open-noscroll', '');
                        document.body.style.overflowY = 'auto';
                    }, duration);
                }
            }
        },
        methods: {
            ok () {
                this.$emit('ok');
                if (this.closeWhenOK) {
                    this.show = false;
                }
            },
            cancel () {
                this.$emit('cancel');
                this.show = false;
            },
            // 点击遮罩层
            clickMask () {
                if (!this.force) {
                    this.cancel();
                }
            }
        }
     };
</script>

<template>
    <div v-show="show" :transition="transition">
        <div class="modal" @click.self="clickMask">
            <div class="modal-dialog" :class="modalClass" v-el:dialog>
                <div class="modal-content">
                    <!--Header-->
                    <div class="modal-header">
                        <slot name="header">
                            <button type="button" class="close" @click="cancel"></button>
                            <h4 class="modal-title">
                                <slot name="title">
                                    {{title}}
                                </slot>
                            </h4>
                        </slot>
                    </div>
                    <!--Container-->
                    <div class="modal-body">
                        <slot></slot>
                    </div>
                    <!--Footer-->
                    <div class="modal-footer">
                        <slot name="footer">
                            <button type="button" :class="cancelClass" @click="cancel">{{cancelText}}</button>
                            <button type="button" :class="okClass" @click="ok">{{okText}}</button>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
        <div class="modal-backdrop in"></div>
    </div>
</template>

<style scoped>
    .modal {
        display: block;
    }
    .modal-transition {
        transition: all .6s ease;
    }
    .modal-leave {
        /* 样式没什么用，但可以让根标签的transitionEnd生效，以去掉modal-leave */
        border-radius: 1px !important;
    }
    .modal-transition .modal-dialog, .modal-transition .modal-backdrop {
        transition: all .5s ease;
    }
    .modal-enter .modal-dialog, .modal-leave .modal-dialog {
        opacity: 0;
        transform: translateY(-30%);
    }
    .modal-enter .modal-backdrop, .modal-leave .modal-backdrop {
        opacity: 0;
    }
</style>