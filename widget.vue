<template>
    <section v-if="mytheme" :style="{ background: mytheme.body_bg}" class="widgetsection s relative fullwidth">
        <slot :selectedMenu__payload="menuPayload" :selectedMenu="currentSelectedMenu" :widgetState="currentWidgetState" name="widget" ></slot>
    </section>
</template>

<script>
import themes from './themes/index'
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
            this.mytheme = themes.light
        } else if(typeof this.theme == 'object') {
            this.mytheme = this.theme
        } else if(typeof this.theme == 'string') {
            this.mytheme = themes[this.theme]
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

<style>
.widget-border {
    /* border: 1px solid #3b485c; */
}
.widgetsection {
  /* background: #26344a; */
  -webkit-box-shadow: 0px 0px 22px -6px rgba(0,0,0,0.71);
  -moz-box-shadow: 0px 0px 22px -6px rgba(0,0,0,0.71);
  box-shadow: 0px 0px 22px -6px rgba(0,0,0,0.71);
  border-radius: 4px !important;
}
</style>