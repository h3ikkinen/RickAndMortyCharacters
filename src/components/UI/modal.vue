<template>
    <transition name="modal">
        <div class="modal">
            <div class="modalOverlay" @click="$emit('close')">
                <div class="modalBody" @click.stop="">
                    <div class="modalHeader">
                        <h1>
                            {{ title }}
                        </h1>
                        <span class="close" @click="$emit('close')">
                            &#10008;
                        </span>
                    </div>
                    <div class="modalContent">
                        <slot name="body">
                            <p>
                                default text
                            </p>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    export default {
        props: {
            title: {
                type: String,
                requred: true,
            }
        },
        mounted () {
            document.body.addEventListener('keyup', (e) => {
                if (e.keyCode === 27) {
                    this.$emit('close');
                }
            });
        }
    }
</script>
<style lang="scss" scoped>
    .modal-enter-active, .modal-leave-active {
        transition: .3s;
        .modalBody {
            transition: .3s;
            transform: scale(1);
        }
    }
    .modal-enter, .modal-leave-to /* .fade-leave-active до версии 2.1.8 */ {
        opacity: 0;
        .modalBody {
            transition: .3s;
            transform: scale(1.2);
        }
    }
    .modal-leave-active {
        .modalBody {
            transition: .3s;
            transform: scale(1.2);
        }
    }
</style>