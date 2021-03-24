<template>
    <v-expand-transition>
        <div id="dq-main-w" v-if="showMenu" class="flex wds">
            <div
                v-if="theme"
                id="dq-host-container"  
                :style="{height:'39px', overflowY:'hidden', overflowX: 'auto', margin:0, padding:0, whiteSpace: 'nowrap'}" 
                :class="['flex', 'ws', 'relative', `widget_tab_bg--${theme}`]" 
            >
                <div class="absolute" >
                    <div class="flex padleft050 padright050" >
                        <div 
                            v-for="(item, item_index) in currentMenus" :key="`${item}-${item_index}`" 
                            
                            :class="['borderRad4 pad025 padleft050 padright050 calItem',
                                selectedMenu == item_index ? `widget_tab_item-active_bg--${theme}` : `widget_tab_item--${theme}`
                            ]"  
                            >
                                <span 
                                    @click.self="selectedMenu = item_index" 
                                    :class="['systemui',
                                        selectedMenu == item_index ? `widget_tab_item-active_color--${theme}` : `widget_tab_item--${theme}`
                                    ]" 
                                >
                                        {{item}}
                                    </span>
                                <v-icon 
                                    @click.self="closeMenu(item), 
                                    gotoMenu(currentMenus[item_index - 1])" 
                                    v-if="closableMenus.includes(item)"
                                    small
                                    :class="[
                                        selectedMenu == item_index ? `widget_tab_item-active_color--${theme}` : `widget_tab_item--${theme}`
                                    ]"
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
        theme: undefined
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
            this.theme = this.$parent.mytheme

        }, 5)
    }
}
</script>

<style>
.calItem:hover {
    cursor: pointer;
}
.calItem{
    min-width: 40px;
}
.ws{
    box-shadow: 0px 16px 15px -3px rgba(5, 5, 5, 0.11);
}
</style>