<template>
    <v-expand-transition>
        <section  class="relative" >
            <section  class="fullwidth borderRad4 flex flexwrap relative" >
                <!-- modal -->
                <transition name="fade2" >
                    <section v-if="modal && show" class="absolute fullwidth fullheight-percent pad125" style="z-index:1" >
                        <div @click="modal = false" class="flex flexcenter flexcol fullheight-percent pad125 relative" >
                            <div @click.stop="" class="widgetsection borderRad4 pad125" 
                                style="max-height:80%; min-width:80%; max-width:80%; overflow:auto; border: 1px solid #3b485c;" >
                                <slot :modalContext="modalContext" name="modal" ></slot>
                            </div>
                        </div>
                    </section>
                </transition>

                <!-- main content -->
                <section id="parent" ref="parent" v-if="show" :class="[show ? 'pad125 fullwidth' : '']"  >
                    <slot  :showModal="showModal" name="widgetContent" ></slot>
                </section>
            </section>
        </section>
    </v-expand-transition>
    
</template>

<script>
export default {
    name: 'WidgetContent',
    directives: {

    },
    data: () => ({
        modal: false,
        modalContext: undefined,
        show: true,
    }),
    methods: {
        showModal(itemContent) {
            this.modal = !this.modal
            this.modalContext = itemContent
        },
        showContent(value) {
            this.show = value
        }
    },
    mounted() {
        setTimeout(() => [
            this.$parent.mountController(this.showContent)
        ], 5)
    }
}
</script>

<style >
.fade2-enter-active, .fade2-leave-active {
  transition: opacity 500ms;
}
.fade2-enter, .fade2-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
.relative{
    transition: 0.3s;
}
</style>