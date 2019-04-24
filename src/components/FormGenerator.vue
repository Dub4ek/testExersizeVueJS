<template>
    <form>
        <v-container v-for="containerIndex in countOfContainers" :key="containerIndex">
            <v-layout>
                <v-text-field v-for="item in getFilteredFormData(containerIndex)" :key="item.id"
                        v-model="item.val"
                        :data-vv-name="item.code"
                        :error-messages="errors.collect(item.code)"
                        :label="item.name"
                        v-validate="getValidationRules(item)"
                        required
                ></v-text-field>
            </v-layout>
        </v-container>
        <v-btn @click="submitButtonClickHandler">submit</v-btn>
    </form>
</template>

<script>
    import Vue from 'vue';
    import VeeValidate from 'vee-validate';
    import { mapMutations } from 'vuex';

    Vue.use(VeeValidate);
    
    export default {
        name: 'FormGenerator',
        $_veeValidate: {
            validator: 'new'
        },
        computed: {
            countOfContainers() {
                return this.formDataCollection ? Object.keys(this.formDataCollection.reduce((prev, cur) => {
                        if (Object.prototype.hasOwnProperty.call(cur, 'parent')) {
                            prev[cur.parent] = null;
                        }
                        return prev;
                    }, {})) : [];
            },
            formDataCollection() {
                return this.$store.state.form;
            }
        },
        methods: {
            getFilteredFormData(value) {
                return this.formDataCollection.filter(item => item.parent === parseInt(value, 10));
            },
            getValidationRules(value) {
                if (Object.prototype.hasOwnProperty.call(value, 'validate')) {
                    return `required|${this.getVeeValidationRule(value.validate)}`;
                }

                return null;
            },
            getVeeValidationRule(value) {
                if (value === 'num') {
                    return 'decimal:10';
                }

                if (value === 'string') {
                    return 'alpha';
                }
            },
            submitButtonClickHandler() {
                this.$validator.validateAll().then(result => {
                    if (result) {
                        this.updateFormData(this.formDataCollection);
                    }
                })
            },
            ...mapMutations(['updateFormData'])
        }
    }
</script>

<style>

</style>
