<template>
    <input
        class="SharpText form-control"
        :type="inputType"
        :value="value"
        :placeholder="placeholder"
        :disabled="readOnly"
        @input="handleInput"
        ref="input"
    >
</template>

<script>
    import { validateTextField } from "../../util/validation";
    import { normalizeText } from "../../util/text";

    export default {
        name:'SharpText',

        props: {
            value: [String, Number],

            placeholder: String,
            readOnly: Boolean,

            maxLength: Number,

            inputType:  {
                type:String,
                default:'text'
            },
        },
        methods: {
            validate(value) {
                return validateTextField(value, {
                    maxlength: this.maxLength,
                });
            },
            handleInput(e) {
                const value = normalizeText(e.target.value);
                const error = this.validate(value);
                this.$emit('input', value, { error });
            },
            focus() {
                this.$refs.input.focus();
            },
        },
    }
</script>
