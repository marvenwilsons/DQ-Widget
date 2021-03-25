<template>
    <section v-if="mytheme" :style="{ overflow:'hidden'}" 
        :class="[ 'widgetsection', 'systemui', 's', 'dashboard-component-wrapper']">
        <slot :selectedMenu__payload="menuPayload" :selectedMenu="currentSelectedMenu" :widgetState="currentWidgetState" name="widget" ></slot>
    </section>
</template>

<script>
export default {
    props: ['show', 'theme'],
    data: () => ({
        childMethod: [],
        currentSelectedMenu: undefined,
        menuPayload: undefined,
        currentWidgetState: undefined,
        mytheme: {}
    }),
    mounted() {
        if(!this.theme) {
            this.mytheme = 'light'
        } else {
            this.mytheme = this.theme
        }
        // mountController is called by child component
        this.mountController = (childMethod) => {
            this.childMethod.push(childMethod)
        }

        this.$on('menuDown', () => {
            this.childMethod.map(e => e(false))
            this.currentWidgetState = false
        })

        this.$on('menuUp', () => {
            this.childMethod.map(e => e(true))
            this.currentWidgetState = true
        })

        this.$on('menuChange', (menu) => {
            this.currentSelectedMenu = menu.selectedMenu
            this.menuPayload = menu.payload
        })

        setTimeout(() => {
            if(this.show == false && typeof this.show == 'boolean') {
                this.currentWidgetState = false
                this.childMethod.map(e => e(false))
            }
        }, 6)
        
    }
}
</script>