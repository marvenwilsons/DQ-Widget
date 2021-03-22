<template>
    <section role="title" class="flex flexcenter pad125 widgetTitle" >
        <slot></slot> 
        <v-icon v-if="state == false" @click="toggle" style="color:#afafaf" class="pointer marginleft050" >mdi-menu-down</v-icon>
        <v-icon v-if="state == true" @click="toggle" style="color:#afafaf" class="pointer marginleft050" >mdi-menu-up</v-icon>
    </section>
</template>

<script>
export default {
    props: ['widgetState'],
    data: () => ({
        state: true
    }),
    mounted() {
        setTimeout(() => {
            if(this.$parent.currentWidgetState == false && typeof this.$parent.currentWidgetState == 'boolean') {
                this.state = false
            }
        },11)
    },
    methods: {
        toggle() {
            this.state = !this.state
            if(this.state == false) {
                this.$parent.$emit('menuDown')
            } else {
                this.$parent.$emit('menuUp')
            }
        }
    }
}
</script>