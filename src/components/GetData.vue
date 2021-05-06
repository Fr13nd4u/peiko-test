<template>
    <div v-if="dataStocksTable" class="table">
        <div class="table-title">
            <strong>Stock</strong>
            <strong>Current</strong>
            <strong>Change</strong>
        </div>
         <ul
            v-for="(item, index) in dataStocksTable"
            :key="index"
         >
            <li>{{ item.stocks }}</li>
            <li>{{ item.current.toFixed(2) }}</li>
            <li
                :class="item.change >= 0 
                    ? 'change-plus' 
                    : 'change-minus'"
            >{{ (+item.change > 0) ? "+" + item.change.toFixed(2) : item.change.toFixed(2) }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: "GetData",
    props: {
        dataStocksTable: Array,
    },
    filters: {
        changeNumberFormat(value) {
            return value.toFixed(2).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
        },
    },
}
</script>

<style scoped lang="scss">

    .table  {
        width: 400px;
        border: 1px solid rgb(240, 240, 240);
        margin: 20px auto;

        &-title {
            width: 100%;
            background-color: rgb(240, 240, 240);
            display: flex;
            justify-content: space-between;

            strong {
                padding: 10px 15px;
            }
        }

        ul {
            width: 100%;
            display: flex;
            justify-content: space-between;
            padding: 0;
            border-top: 1px solid rgb(240, 240, 240);
            margin: 0px;

            li {
                list-style: none;
                padding:  15px;
            }
        }

    }

    .isClicked {
        transform: scale(0.95);
        border: 1px solid grey;
    }

    .change-plus {
        color: red;
    }
    .change-minus {
    color: green;
    }

</style>