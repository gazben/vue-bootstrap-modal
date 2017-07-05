# vue-bootstrap-modal

Bootstrap style modal component for vue.js 2.

# Usage

In the main.js file:
```
require('bootstrap-sass')
import VueModal from '../src/components/helpers/modal/modal.vue'
Vue.component('modal', VueModal)
```


``` html
<modal title="Modal Title" :show.sync="show" @ok="ok" @cancel="cancel" :body="This will be the body"
</modal>
```

#Props
``` javascript
props: {
    show: {
        type: Boolean,
        twoWay: true,
        default: false
    },
    title: {
        type: String,
        default: 'Modal'
    },
    // This text will be shown in the modal body
    body: {
        type: String
    },
    // Bootstrap small style modal
    small: {
        type: Boolean,
        default: false
    },
    // Bootstrap large style modal
    large: {
        type: Boolean,
        default: false
    },
    // Bootstrap full style modal    
    full: {
        type: Boolean,
        default: false
    },
    // if it set false, click background will close modal 
    force: {
        type: Boolean,
        default: false
    },
    // vue transition name
    transition: {
        type: String,
        default: 'modal'
    },
    // [OK button] text
    okText: {
        type: String,
        default: 'OK'
    },
    // [Cancel button] text
    cancelText: {
        type: String,
        default: 'Cancel'
    },
    // [OK button] className
    okClass: {
        type: String,
        default: 'btn btn-primary'
    },
    // [Cancel button] className
    cancelClass: {
        type: String,
        default: 'btn btn-warning'
    },
    // automatically close when click [OK button]
    closeWhenOK: {
        type: Boolean,
        default: false
    }
}
```

# License
MIT