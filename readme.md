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
- `Widget`
- `WidgetContent`
- `WidgetTitle`
- `WidgetMenu`

# Widget Fixed tabs
```js
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
    - Template: `<WidgetMenu ref="refName" />`
    - Script: `this.refs.refName`
2. Invoke the `createMenu` method
```js
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