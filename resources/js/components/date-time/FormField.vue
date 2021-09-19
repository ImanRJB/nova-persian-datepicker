<template>
        <default-field :field="field" :errors="errors">
            <template slot="field">
                <date-picker
                    :id="field.name"
                    type="datetime"
                    :class="[errorClasses, {'nullable': field.nullable}]"
                    inputClass="w-full form-control form-input form-input-bordered date-ltr"
                    :format="format"
                    :initial-value="persianDate"
                    v-model="value"
                >

                </date-picker>

                <a v-if="field.nullable" @click="removeValue" title="Clear value" tabindex="-1"
                   style="vertical-align: text-bottom"
                   class="p-1 px-2 cursor-pointer leading-none focus:outline-none">
                    <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 22 22"
                         aria-labelledby="x-circle" role="presentation" class="fill-current">
                        <path
                            d="M4.93 19.07A10 10 0 1 1 19.07 4.93 10 10 0 0 1 4.93 19.07zm1.41-1.41A8 8 0 1 0 17.66 6.34 8 8 0 0 0 6.34 17.66zM13.41 12l1.42 1.41a1 1 0 1 1-1.42 1.42L12 13.4l-1.41 1.42a1 1 0 1 1-1.42-1.42L10.6 12l-1.42-1.41a1 1 0 1 1 1.42-1.42L12 10.6l1.41-1.42a1 1 0 1 1 1.42 1.42L13.4 12z"></path>
                    </svg>
                </a>
            </template>
        </default-field>
</template>

<script>
import VuePersianDatetimePicker from 'vue-persian-datetime-picker'
import jMoment from "moment-jalaali"
import {FormField, HandlesValidationErrors} from 'laravel-nova'

export default {
    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],
    computed: {
        format() {
            return this.field.format || 'jYYYY/jMM/jDD HH:mm:ss';
        },
        persianDate() {
            if (!this.field.value) {
                return '';

            } else {
                return jMoment(this.field.value).format(this.format);
            }

        },
        placeholder() {
            return this.field.placeholder || jMoment().format(this.format)
        },
        altDateValue() {
            return this.value ? jMoment(this.value, this.format).format('YYYY-MM-DD HH:mm:ss') : '';
        }
    },
    methods: {
        setInitialValue() {
            this.value = this.persianDate;
        },

        fill(formData) {
            formData.append(this.field.attribute, this.altDateValue || '');
        },
        removeValue() {
            this.value = '';
            this.persianDate = '';
        }
    },
    components: {
        datePicker: VuePersianDatetimePicker
    }
}
</script>
<style>
.date-ltr {
    direction: ltr !important;
    text-align: right;
}

.nullable {
    display: inline-table !important;
    width: 90%
}
</style>
