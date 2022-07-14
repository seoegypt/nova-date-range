<template>
    <DefaultField :field="field" :errors="errors" :show-help-text="showHelpText">
        <template #field>
            <date-range-picker
                class="w-full form-control form-input form-input-bordered"
                mode="sa"
                :name="field.name"
                :field="field"
                :value="value"
                :seperator="seperator"
                :firstDayOfWeek="firstDayOfWeek"
                :dateFormat="format"
                :placeholder="placeholder"
                @change="handleChange"
                :disabled="isReadonly"
            />

            <p v-if="hasError" class="my-2 text-danger">
                {{ firstError }}
            </p>
        </template>
    </DefaultField>
</template>

<script>
import moment from 'moment';
import DateRangePicker from './DateRangePicker'
import { FormField, HandlesValidationErrors } from 'laravel-nova'

export default {
    props: ['resourceName', 'resourceId', 'field'],
    mixins: [HandlesValidationErrors, FormField],
    components: { DateRangePicker },
    methods: {
        /*
         * Set the initial, internal value for the field.
         */
        setInitialValue() {
            this.value = this.field.value || ''
        },

        /**
         * Fill the given FormData object with the field's internal value.
         */
        fill(formData) {
            formData.append(this.field.attribute, this.value || '')
        },
    },
    computed: {
        format() {
            return this.field.format
        },
        seperator() {
            return this.field.seperator
        },
        firstDayOfWeek() {
            return this.field.firstDayOfWeek || 0;
        },
        placeholder() {
            return moment().format('YYYY-MM-DD') + ` ${this.field.seperator} ` + moment().format('YYYY-MM-DD')
        },
    },
}
</script>
