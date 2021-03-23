<template>
    <section role="title" v-if="titleTheme" :style="{background: titleTheme.title_bg, color: titleTheme.title_color}" class="flex flexcenter pad125 widgetTitle" >
        <slot ></slot> 
        <v-icon 
            v-if="state == false" 
            @click="toggle" 
            :style="{color:titleTheme.title_color}" 
            class="pointer marginleft050" >
                mdi-menu-down
        </v-icon>
        <v-icon 
            v-if="state == true" 
            @click="toggle" 
            :style="{color:titleTheme.title_color}" 
            class="pointer marginleft050" >
                mdi-menu-up
        </v-icon>
    </section>
</template>

<script>
export default {
    props: ['widgetState'],
    data: () => ({
        state: true,
        titleTheme: undefined
    }),
    mounted() {
        setTimeout(() => {
            this.titleTheme = this.$parent.mytheme
            if(this.$parent.currentWidgetState == false && typeof this.$parent.currentWidgetState == 'boolean') {
                this.titleTheme = this.$parent.mytheme
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