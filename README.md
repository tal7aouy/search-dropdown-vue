# search-dropdown-vue

An easier way to display A Vue.js dropdown component with search,
No special dependencies, no jquery, no tailwind.css, just VueJS and CSS magic.

# Installation

```bash
$ npm install search-dropdown-vue
// OR
$ yarn add  search-dropdown-vue
```

# Requirements

- [Vue.js](https://github.com/vuejs/vue-next) `^3.0.0`

# Usage

```html
<SearchDropdown
   :options="countries"
    v-on:selected="onSelectedOption"
    v-on:filter="getDropdownValues"
>
</SearchDropdown>

<script setup>
  import SearchDropdown from 'search-dropdown-vue'
  import {reactive, ref} from 'vue
  const countries = ref([
      {id:1, name:'Morocco'},
      {id:2, name:'USA'},
      {id:3,name: "Canada"}
    ])
  let object = reactive({id:null, name: null})

  const onSelectedOption = (payload) => object = payload
</script>
```

# Default values of props

| Property            |  Type   |              Default value |
| ------------------- | :-----: | -------------------------: |
| closeOnOutsideClick | boolean |                       true |
| placeholder         | string  | 'Please select an option.' |
| name                | string  |          'dropdown input.' |
| disabled            | boolean |                      false |
| maxItem             | number  |                         10 |

# License

[The MIT License](http://opensource.org/licenses/MIT)
