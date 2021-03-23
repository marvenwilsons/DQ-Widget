![ Alt text](Demo.gif)
# Installation
### Nuxt.js
- Create a new directory in components if doesn't exist, named globals.
- Register the components in plugins, by creating `widget.js` file, then import widget files.
```js
// plugins/widget.js
import Vue from "vue";

import Widget from          '@/components/globals/widget/widget'
import WidgetModal from     '@/components/globals/widget/widget-modal'
import WidgetContent from   '@/components/globals/widget/widget-content'
import WidgetTitle from     '@/components/globals/widget/widget-title'
import WidgetBody from      '@/components/globals/widget/widget-body'
import WidgetMenu from      '@/components/globals/widget/widget-menu'

Vue.component("Widget", Widget);
Vue.component("WidgetModal", WidgetModal);
Vue.component("WidgetSection", WidgetContent);
Vue.component("WidgetTitle", WidgetTitle);
Vue.component("WidgetBody", WidgetBody);
Vue.component("WidgetMenu", WidgetMenu);
```

```js
//nuxt.config.js
module.exports = {
    plugins: [
        { src: '~/plugins/widget.js', mode: 'client' },
    ],
}
```
### Vue.js
- for plain vue scafolding, do the same steps taken in nuxt.js instructions, then you can import each components in your custom components.

# Widget Features
- tabs
- closable tabs
- widget modal
- widget modal context
- widgetContext

# Widget components
- `<Widget/>` - Wrapper
- `<WidgetContent/>` - The main content of the widget
- `<WidgetTitle/>` - Display's title on the top of the widget
- `<WidgetMenu/>` - Display's tabs

# Widget Fixed tabs
```html
<template>
    <div>
        <Widget>
            <template #widget="{selectedMenu, selectedMenu__payload}" >
                <WidgetTitle> Sample title </WidgetTitle>
                <WidgetMenu :menus="['foo','bar']" :defaultSelected="0" />
                <WidgetContent>
                    <template #widgetContent  >
                        {{selectedMenu}} <!-- foo -->
                    </template>
                </WidgetContent>
            </template>
        </Widget>
    </div>
</template>

<script>
export default{}
</script>
```

# Widget with Dynamic & Closable tabs
1. Create a reference to `WidgetMenu` component
    - In Template: `<WidgetMenu ref="refName" />`
    - In Script: `this.$refs.refName`
2. Invoke the `createMenu(<config & payload:Object>)` method
```html
<template>
    <div>
        <Widget>
            <template #widget="{selectedMenu, selectedMenu__payload}" >
                <WidgetTitle> Sample title </WidgetTitle>
                <WidgetMenu ref="WidgetMenu" :menus="['foo','bar']" :defaultSelected="0" />
                <WidgetContent>
                    <template #widgetContent  >
                        {{selectedMenu}} <!-- tab name, because of isActive -->
                        {{selectedMenu__payload}} <!-- you can now use this for render condition -->
                    </template>
                </WidgetContent>
            </template>
        </Widget>
    </div>
</template>

<script>
export default{
    mounted() {
        const WigetMenuReference = this.refs.WidgetMenu
        WigetMenuReference.createMenu({
            name: 'tab name',
            payload: {}, // can be access in selectedMenu__payload in template
            isActive: true,
            isClosable: true
        })
    }
}
</script>
```
# Using widget modal
1. Create a reference to `WigetContent` component
    - In Template `<WidgetContent ref="WidgetContent" />`
    - In Script `this.$refs.WidgetContent`
2. Invoke `showModal(<payload:any>)` method
    - payload is accessable in `modalContext` in tamplate

```html
<template>
    <div>
        <Widget>
            <template #widget="{selectedMenu, selectedMenu__payload}" >
                <WidgetTitle> Sample title </WidgetTitle>
                <WidgetMenu :menus="['foo','bar']" :defaultSelected="0" />
                <WidgetContent ref="WidgetContent" >
                    <!-- modal here -->
                    <template v-if="true" #modal="{modalContext}" >
                        {{modalContext}} <!-- it will print: pass anything here. -->
                    </template>
                    <template #widgetContent  >
                        {{selectedMenu}} <!-- foo -->
                    </template>
                </WidgetContent>
            </template>
        </Widget>
    </div>
</template>

<script>
export default{
    mounted() {
        const WidgetContentReference = this.refs.WidgetContent
        WidgetContentReference.showModal("pass anything here.")
    }
}
</script>
```
3. to close the modal programatically, call the `showModal()` again, it toggles from `true` and `false` every method call.
    - the modal can also be closed by clicking outside of the modal.

# Using Themes and custom colors
todo