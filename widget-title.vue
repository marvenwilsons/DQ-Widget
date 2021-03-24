<template>
    <section role="title" v-if="theme" 
        :class="['flex flexcenter pad125 widgetTitle', `widget_title_color--${theme}`]" >
        <slot ></slot> 
        <v-icon 
            v-if="state == false" 
            @click="toggle" 
            :class="['pointer marginleft050',`widget_title_color--${theme}`]" >
                mdi-menu-down
        </v-icon>
        <v-icon 
            v-if="state == true" 
            @click="toggle" 
            :class="['pointer marginleft050',`widget_title_color--${theme}`]" >
                mdi-menu-up
        </v-icon>
    </section>
</template>

<script>
export default {
    props: ['widgetState'],
    data: () => ({
        state: true,
        theme: undefined
    }),
    mounted() {
        setTimeout(() => {
            this.theme = this.$parent.mytheme
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