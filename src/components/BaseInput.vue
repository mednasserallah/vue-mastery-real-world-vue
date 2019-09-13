<template>
    <div class="field">
        <label>{{ label }}</label>
        <input :value="value" @input="updateValue" v-bind="$attrs" />
    </div>
</template>

<script>
export default {
    inheritAttrs: false,
    props: {
        label: {
            type: String,
            default: ''
        },
        value: {
            type: [String, Number],
            required: false
        }
    },
    methods: {
        updateValue() {
            this.$emit('input', event.target.value);
        }
    }
};
</script>
