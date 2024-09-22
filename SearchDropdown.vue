<script setup>
import { ref, computed, watch } from 'vue';

// Props
const props = defineProps({
  name: {
    type: String,
    required: false,
    default: 'input',
  },
  options: {
    type: Array,
    required: true,
    default: () => [],
  },
  placeholder: {
    type: String,
    required: false,
    default: 'Please select an option',
  },
  disabled: {
    type: Boolean,
    required: false,
    default: false,
  },
  maxItem: {
    type: Number,
    required: false,
    default: 10,
  },
});

// Local state
const selected = ref({});
const optionsShown = ref(false);
const searchFilter = ref('');

// Computed properties
const filteredOptions = computed(() => {
  const filtered = [];
  const lowerCaseSearch = searchFilter.value.toLowerCase(); // Convert search input to lowercase

  for (const option of props.options) {
    const optionNameLowerCase = option.name.toLowerCase(); // Convert option name to lowercase
    // Use includes() to check for matches
    if (
      lowerCaseSearch.length < 1 ||
      optionNameLowerCase.includes(lowerCaseSearch)
    ) {
      if (filtered.length < props.maxItem) {
        filtered.push(option);
      }
    }
  }

  return filtered;
});

// Emits
const emit = defineEmits(['selected', 'filter']);

// Watch searchFilter changes
watch(searchFilter, () => {
  if (filteredOptions.value.length === 0) {
    selected.value = {};
  } else {
    selected.value = filteredOptions.value[0];
  }
  emit('filter', searchFilter.value);
});

// Functions
function selectOption(option) {
  selected.value = option;
  optionsShown.value = false;
  searchFilter.value = selected.value.name;
  emit('selected', selected.value);
}

function showOptions() {
  if (!props.disabled) {
    searchFilter.value = '';
    optionsShown.value = true;
  }
}

function exit() {
  if (!selected.value.id) {
    selected.value = {};
    searchFilter.value = '';
  } else {
    searchFilter.value = selected.value.name;
  }
  optionsShown.value = false;
}

function keyMonitor(event) {
  if (event.key === 'Enter' && filteredOptions.value[0]) {
    selectOption(filteredOptions.value[0]);
  }
}
</script>

<template>
  <div class="dropdown" v-if="props.options">
    <div class="dropdown-toggle">
      <input
        :name="props.name"
        @focus="showOptions"
        @blur="exit"
        @keyup="keyMonitor"
        v-model="searchFilter"
        :disabled="props.disabled"
        :placeholder="props.placeholder"
      />
    </div>
    <transition name="fade">
      <ul class="dropdown-menu" v-show="optionsShown">
        <li
          @mousedown="selectOption(option)"
          v-for="(option, index) in filteredOptions"
          :key="index"
        >
          <a href="javascript:void(0)">
            {{ option.name || option.id || '-' }}
          </a>
        </li>
      </ul>
    </transition>
  </div>
</template>


<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 250ms cubic-bezier(0.25, 0.46, 0.45, 0.94) 0s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.dropdown {
  min-width: 160px;
  height: 40px;
  position: relative;
  margin: 10px 1px;
  display: inline-block;
  vertical-align: middle;
}
.dropdown a:hover {
  text-decoration: none;
}

.dropdown-toggle {
  position: relative;
  display: inline-block;
  transition: 0.75s ease-in;
  border-radius: 2px 2px 0 0;
}
.dropdown-toggle input {
  border: none;
  background: #0002;
  padding: 0.5rem;
  outline: none;
  transition: 0.75s ease-in;
  color: gray;
  border-radius: 4px;
}

.dropdown-toggle:hover input {
  background: #0004;
  transition: 0.3s;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  float: left;
  min-width: 160px;
  width: 100%;
  padding: 10px 20px 10px 10px;
  margin: 0 0 0;
  list-style: none;
  font-size: 14px;
  text-align: left;
  background-color: #fff;
  border: 1px solid #ccc;
  border-top: none;
  border-radius: 4px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  background-clip: padding-box;
}

.dropdown-menu > li > a {
  padding: 10px 20px;
  display: block;
  clear: both;
  font-weight: normal;
  line-height: 1.6;
  color: #333333;
  white-space: nowrap;
  text-decoration: none;
  max-width: 100%;
}
.dropdown-menu > li > a:hover {
  background: #efefef;
  color: #409fcb;
  border-radius: 4px;
}

.dropdown-menu > li {
  overflow: hidden;
  position: relative;
  margin: 0;
}

li {
  list-style: none;
}
</style>
