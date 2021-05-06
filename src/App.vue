<template>
    <div id="app">
        <button
            @click="getDataStocks()"
            :disabled="loading"
            class="btn"
        >
            Get data
        </button>
        <div v-if="isLoading" class="loading">
            ...loading
        </div>
        <div v-else>
            <div v-if="requestFailed" class="error">
                no data
            </div>
            <GetData
                v-else
                :data-stocks-table="dataStocksTable"
            />
        </div>
    </div>
</template>

<script>
import {payload} from "@/mocData";
import simulateAsyncReq from "@/plugins/getDataFunc";
import GetData from "@/components/GetData";
export default {
    name: 'App',
    data() {
        return {
            dataStocksTable: null,
            requestFailed: false,
            isLoading: false,
        }
    },
    methods: {
        getDataStocks() {
            this.isLoading = true;

            simulateAsyncReq(payload)
                .then(data => {
                    let arrayObjStocks = [];

                    for (let i = 0; i < data.stocks.length; i++) {
                        let objStock = {};
                        Object.keys(data).forEach(item => objStock[item] = data[item][i]);
                        if (objStock.current && objStock.start) {
                            objStock.change = objStock.current - objStock.start;
                            delete objStock.start
                            arrayObjStocks.push(objStock);
                        }
                    }
                    
                    this.dataStocksTable = this.sortByAlphabet(arrayObjStocks);
                    this.requestFailed = false;
                })
                .catch(() => this.requestFailed = true)
                .finally(() => this.isLoading = false)
        },
        
        sortByAlphabet(array) {
            return array.sort((a, b) => {
                let firstStock = a.stocks.toLowerCase();
                let secondStock = b.stocks.toLowerCase();
                return firstStock < secondStock ? -1 : firstStock > secondStock ? 1 : 0
            });
        }
    },
    components: {
      GetData
    }
}
</script>

<style lang="scss">
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
  border: 1px solid #ccc;
  width: 150px;
  padding: 10px;
  font-size: 16px;
  font-weight: 600;
  &:hover{
    background: #fff;
  }
}
.loading {
  margin-top: 20px;
  font-size: 25px;
}
.error {
  margin-top: 20px;
  font-size: 25px;
  color: red;
}
</style>