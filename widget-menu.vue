<template>
    <v-expand-transition>
        <div id="dq-main-w" v-if="showMenu" class="padleft125 padright125 flex">
            <div
                v-if="tabMenu_theme"
                id="dq-host-container"  
                :style="{height:'39px', overflowY:'hidden', overflowX: 'auto', margin:0, padding:0, whiteSpace: 'nowrap', background: tabMenu_theme.tab_bg}" 
                class="flex widgetsection relative" 
            >
                <div class="absolute" >
                    <div class="flex" >
                        <div 
                            v-for="(item, item_index) in currentMenus" :key="`${item}-${item_index}`" 
                            @click.self="selectedMenu = item_index" 
                            :class="['borderRad4', 'pad025', 'padleft050', 'padright050', 'calItem', 
                            selectedMenu == item_index ? '' : '']" 
                            :style="{
                                border: `1px solid ${tabMenu_theme.tab_item_border_color}`, 
                                background: selectedMenu == item_index ? tabMenu_theme.tab_item_active_bg : '',
                                color: tabMenu_theme.tab_item_active_color
                            }" 
                            >
                                {{item}}
                                <v-icon 
                                    @click.self="closeMenu(item), 
                                    gotoMenu(currentMenus[item_index - 1])" 
                                    v-if="closableMenus.includes(item)" 
                                    :style="{color: tabMenu_theme.tab_item_active_color}"
                                    >
                                    mdi-close
                                </v-icon>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </v-expand-transition>
</template>
<script>
export default {
    props: ['menus', 'defaultSelected', 'closable'],
    data: () => ({
        selectedMenu: undefined,
        currentMenus: [],
        showMenu: true,
        closableMenus: [],
        payload: [],
        tabMenu_theme: undefined
    }),
    watch: {
        selectedMenu(index) {
            if(this.currentMenus[index] != undefined) {
                this.$parent.$emit('menuChange',{
                    selectedMenu: this.currentMenus[this.selectedMenu],
                    payload: this.payload[index]
                })
            }
        }
    },
    methods: {
        showMenuController(value) {
            this.showMenu = value
        },
        autoScroll() {
            // auto scroll
            const paneContainer = document.getElementById('dq-main-w')
            const paneContainerWidth = paneContainer.offsetWidth
            const hostContainer = document.getElementById('dq-host-container')
            const hostContainerWidth = Math.round(hostContainer.getBoundingClientRect().width)
            if(paneContainerWidth > hostContainerWidth) {
                setTimeout(() => {
                    hostContainer.scrollTo({ top: 0, left: paneContainerWidth * 10, behavior: 'smooth' })
                },50)
            }
        },
        gotoMenu(menuName) {
            
            this.selectedMenu = this.currentMenus.indexOf(menuName)
        },
        closeMenu(item) {
            const getItemIndex = this.currentMenus.indexOf(item)
            this.currentMenus.splice(getItemIndex, 1)
            this.payload.splice(getItemIndex, 1)
        },
        createMenu(menuInfo) {
            const createUniqueName = (name) => {
                const create = (_name) => {
                    if(this.currentMenus.includes(_name) == true) {
                        const getNewNameNum = _name.split('_')
                        
                        if(getNewNameNum.length == 1) {
                            return create(`${_name}_0`)
                        } else {
                            const newName = `${getNewNameNum[0]}_${parseInt(getNewNameNum[1]) + 1}`
                            return create(newName)
                        }
                    } else {
                        return `${_name}`
                    }
                }
                return create(name)
            }

            if(typeof menuInfo == 'string') {
                this.currentMenus.push(menuInfo)
            } else if(typeof menuInfo == 'object' && !Array.isArray(menuInfo)) {
                const name = createUniqueName(menuInfo.name)
                this.currentMenus.push(name)
                
                if(menuInfo.isActive == true) {
                    this.gotoMenu(name)
                }

                if(menuInfo.isClosable == true) {
                    this.closableMenus.push(name)
                }

                if(menuInfo.payload) {
                    this.payload.push(menuInfo.payload)
                }

                this.autoScroll()
            }
        }
    },
    mounted() {
        if(this.defaultSelected != undefined) {
            this.selectedMenu = this.defaultSelected
        } else {
            this.selectedMenu = 0
        }

        this.currentMenus = this.menus
        this.currentMenus.map(e => {
            this.payload.push(e)
        })

        setTimeout(() => {
            this.$parent.mountController(this.showMenuController)
            this.tabMenu_theme = this.$parent.mytheme

        }, 5)
    }
}
</script>

<style>
.calItem:hover {
    /* background: orange; */
    /* color: #333; */
    cursor: pointer;
}
.calItem{
    min-width: 40px;
}
</style>