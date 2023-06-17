<script setup>
import {ref, computed, watch, onMounted} from 'vue'

const valid = ref(null)
const inputCtrl = ref(null)

const props = defineProps({
    modelValue: { type: Object },
    type: { type: String, default: "text" },
    maxLength: { type: String, default: "" },
    placeholder: { type: String, default: "" },
    pattern: { type: [String, undefined], default: undefined },
    required: { type: Boolean, default: false },
})

const emit = defineEmits(['update:modelValue', 'valid'])

const onInput = (e) => {
    // console.log("onInput: value =", e.target.value)
    emit('update:modelValue', e.target.value)

    if(e.target.value.length === 0) {
        valid.value = null
        emit('valid', valid.value)
    }
    else {
        valid.value = e.target.checkValidity()
        emit('valid', valid.value)
    }
}

const onBlur = (e) => {
    valid.value = e.target.checkValidity()
    emit('valid', valid.value)

    if(!valid.value) {
        e.target.reportValidity()
    }
}

const validityClass = computed(() => {
    if(valid.value === true) return "is-valid"
    if(valid.value === false) return "is-invalid"
    return ""
})

onMounted(() => {
    if(inputCtrl.value.checkValidity()) {
        valid.value = true
        emit('valid', valid.value)
    }
})

watch(() => props.modelValue, (newValue) => {
    // console.log("Watcher:", newValue)
    if(newValue.length === 0)
        valid.value = null
})
</script>

<template>
    <input ref="inputCtrl"
        class="form-control form-control-lg"
        :class="validityClass"
        :type="props.type"
        :maxlength="maxLength"
        :placeholder="props.placeholder"
        :pattern="props.pattern"
        :required="props.required"

        :value="props.modelValue"
        @input="onInput($event)"

        @blur="onBlur($event)"
    />
</template>