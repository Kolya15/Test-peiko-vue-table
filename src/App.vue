<template>
    <div id="app">
        <button
            @click="getDataStocks()"
            :disabled="loading"
            class="btn"
        >
            Get data
        </button>
        <div v-if="loading" class="information-tip">
            loading...
        </div>
        <div v-else>
            <div v-if="requestFailed" class="information-tip information-tip__error">
                no data
            </div>
            <TableOfStocks
                v-else
                :data-stocks-table="dataStocksTable"
            />
        </div>
    </div>
</template>

<script>
import {payload} from "@/mocData";
import simulateAsyncReq from "@/plugins/getDataFunc";
import TableOfStocks from "@/components/TableOfStocks";

export default {
    name: 'App',
    data() {
        return {
            dataStocksTable: null,
            requestFailed: false,
            loading: false,
        }
    },
    methods: {
        getDataStocks() {
            this.loading = true;
            simulateAsyncReq(payload)
                .then(data => {
                    let arrayObjStocks = [];
                    for (let i = 0; i < Object.keys(data).length; i++) {
                        let objStock = {};
                        Object.keys(data).forEach(item => objStock[item] = data[item][i]);
                        objStock.change = objStock.current - objStock.start;
                        arrayObjStocks.push(objStock);
                    }
                    this.dataStocksTable = this.sortByAlphabet(arrayObjStocks);
                    this.requestFailed = false;
                })
                .catch(() => this.requestFailed = true)
                .finally(() => this.loading = false)
        },
        sortByAlphabet(array) {
            return array.sort((a, b) => a.stocks < b.stocks ? -1 : a.stocks > b.stocks ? 1 : 0);
        }
    },
    components: {
        TableOfStocks
    }
}
</script>

<style lang="scss">
@import "scss/var";

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin-top: 60px;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-sizing: border-box;
}

.btn {
    background-color: $background-color;
    border: $border;
    border-radius: $border-radius;
    padding: 10px 40px;
    font-size: 20px;
    margin-bottom: 40px;
    outline: none;
    cursor: pointer;
}

.information-tip {
    font-size: 25px;

    &__error {
        color: $color-error;
    }
}

</style>
