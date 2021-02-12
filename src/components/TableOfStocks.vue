<template>
    <div v-if="dataStocksTable" class="stocks-table">
        <div class="stocks-table__header">
            <div>Stock</div>
            <div>Current</div>
            <div>Change</div>
        </div>
        <div
            v-for="(item, index) in dataStocksTable"
            :key="index"
            class="stocks-table__body"
        >
            <div>{{ item.stocks }}</div>
            <div>{{ item.current | changeNumberFormat }}</div>
            <div :class="item.change >= 0 ? 'stocks-table__body_positive-value' : 'stocks-table__body_negative-value'">
                {{ item.change | changeNumberFormat | addingPlus }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "TableOfStocks",
    props: {
        dataStocksTable: Array,
    },
    filters: {
        changeNumberFormat(value) {
            return value.toFixed(2).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        },
        addingPlus(value) {
            return value >= 0 ? `+${value}` : value;
        }
    },
}
</script>

<style scoped lang="scss">

@import "src/scss/var";

%table-row {
    display: flex;
    justify-content: center;
    width: 100%;
    font-size: 16px;

    div {
        width: 33.3%;
        padding: 20px;
    }
}

.stocks-table {
    width: 400px;
    border: $border;
    border-radius: $border-radius;

    &__header {
        @extend %table-row;
        font-weight: 800;
        background-color: $background-color;
        border-top-left-radius: $border-radius;
        border-top-right-radius: $border-radius;
    }

    &__body {
        @extend %table-row;
        border-top: $border;

        &_positive-value {
            position: relative;
            color: #65d242;
        }

        &_negative-value {
            color: $color-error;
        }
    }
}

@media (max-width: 450px) {
    %table-row {
        font-size: 14px;
    }

    .stocks-table {
        width: 280px;
    }
}

</style>