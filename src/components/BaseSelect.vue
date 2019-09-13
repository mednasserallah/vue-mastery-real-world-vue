<template>
    <div class="field">
        <label>{{ label }}</label>
        <select :value="value" @input="updateValue" v-bind="$attrs">
            <option
                v-for="option in options"
                :key="option"
                :selected="option === value"
            >{{ option }}</option>
        </select>
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
        options: {
            type: Array,
            required: false
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
