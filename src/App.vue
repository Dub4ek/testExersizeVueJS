<template>
    <v-app>
        <v-content>
            <FormGenerator/>
        </v-content>
    </v-app>
</template>

<script>
    import Vue from 'vue';
    import FormGenerator from './components/FormGenerator'
    import Vuex from 'vuex';
    Vue.use(Vuex);


    const store = new Vuex.Store({
        state: {
            form:[
                {
                    "id": "1",
                    "name": "Заказ",
                    "code": "order",
                    "type": "group",
                    "orderID": 1,
                    "parent": 0,
                    "val": "all"
                },
                {
                    "id": "2",
                    "name": " Габаритность ",
                    "code": "gab",
                    "type": "input",
                    "orderID": 2,
                    "parent": 1,
                    "val": "rps",
                },
                {
                    "id": "3",
                    "name": " тоннажность ",
                    "code": "ton",
                    "type": "input",
                    "orderID": 1,
                    "parent": 1,
                    "val": 2,
                    "validate": "num"
                },
                {
                    "id": "4",
                    "name": "Станция",
                    "code": "station",
                    "type": "input",
                    "orderID": 1,
                    "parent": 1,
                    "val": 2,
                    "validate": "string"
                }
            ]
        },
        mutations: {
            updateFormData(state, value = []) {
                state.form = [...value]
            }
        }
    });

    export default {
        name: 'App',
        store,
        components: {
            FormGenerator
        }
    }
</script>
