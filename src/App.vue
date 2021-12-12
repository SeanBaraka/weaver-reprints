<template>
  <main class="flexed">
    <section id="search" style="margin-top:70px">
      <div class="logo">
        <img alt="Artemis" src="./assets/img/applogo.png">
      </div>
      <div class="top-header">
        <div class="flexed">
          <img src="./assets/img/line.svg" alt="">
          <img id="check-img" src="./assets/img/active-check.svg" alt="">
          <p id="p-content">Search Sale Record</p>
        </div>
      </div>
      <div class="search-form">
        <div class="flexed">
          <div id="shop-search">
            <art-input :value="searchParams.shop" placeholder="Select Shop" label="Shop Name" />
          </div>
          <div id="receipt-search">
            <art-input label="Receipt Number" placeholder="Enter the receipt number"  @on-change="searchParams.receiptNumber = $event.target.value" />
          </div>
        </div>
        <div class="search-submit">
          <art-button class="button-secondary" @on-clicked="searchReceipt()">
            <span v-if="!searchLoading">Search Record</span>
            <art-loader label="Searching" v-else />
          </art-button>
          <span v-if="error !== ''">{{error}}</span>
        </div>
      </div>
    </section>

    <section id="receipt" class="receipt" style="margin-top: 10px">
      <div class="receipt-box" :class="receipt.id != undefined ? 'with-content': 'no-content'">
        <div class="receipt-box-card" v-if="receipt.id != undefined">
          <div class="receipt-box-card-contents">
            <div class="header">
              <p>WEAVERBIRD SUPPLIES LTD</p>
              <p>P.O. BOX 300 - 90100, MKS</p>
              <p>TELEPHONE NO. 0727275937</p>
              <p>VAT #: 01002345P</p>
              <p>PIN: P05435RC3</p>
            </div>
            <div class="sub-header">
              <p>Cash Sale: #{{receipt.receiptNumber}}</p>
              <p>Date Time: {{receipt.date}} {{receipt.time}}</p>
              <p>Store: WEAVER</p>
            </div>
            <div class="items">
              <div class="items-head grid">
                <p>ITEM</p>
                <p>QTY</p>
                <p>PRICE</p>
                <p class="last">AMOUNT</p>
              </div>
              <div class="items-body">
                <div class="grid"  v-for="item of items" :key="item.id">
                  <p>{{item.name}}</p>
                  <p>{{item.quantity}}</p>
                  <p>{{item.sellingPrice}}</p>
                  <p class="last">{{(item.quantity * item.sellingPrice).toFixed(2)}}</p>
                </div>
              </div>
              <div class="items-footer">
                <div class="flexed">
                  <p>TOTAL AMT</p>
                  <p>{{receipt.saleAmount.toFixed(2)}}</p>
                </div>
              </div>
              <div class="items-bottom">
                <p>You were served By: Peter Mutune</p>
                <p class="tiny">Thank you, glad to have served you. welcome again</p>
              </div>
            </div>
          </div>
        </div>
        <div class="receipt-no-content" v-else>
          <div class="inner">
            <img src="@/assets/img/scan-icon.svg" width="70" alt="">
            <p>Could not find a sale record</p>
            <div class="search-submit">
              <art-button class="inactive">Retry Search</art-button>
            </div>
          </div>
        </div>
      </div>
      <div class="print-container" style="margin:1em 0" v-if="receipt.id != undefined">
        <art-button class="button-primary button-small">PRINT</art-button>
      </div>
    </section>
  </main>
  
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ArtButton from './components/ui-components/ArtButton.vue';
import ArtInput from './components/ui-components/ArtInput.vue';
import ArtLoader from './components/ui-components/ArtLoader.vue';
import axios from 'axios'

export default defineComponent({
  name: 'App',
  components: {
    ArtInput,
    ArtButton,
    ArtLoader
  },
  data() {
    return {
      receipt: {} as any,
      items: [] as any[],
      error: '',
      searchLoading: false,
      searchParams: {
        shop: '2',
        receiptNumber: ''
      }
    }
  },
  mounted() {

  },
  methods: {
    doSomething(s: any) {
      console.log(s)
    },
    searchReceipt() {
      console.log(typeof(this.receipt))
      this.searchLoading = true;
      const shopId = this.searchParams.shop;
      const receiptNumber = this.searchParams.receiptNumber;
    
      axios.post('https://api-v2.weaverbirdsupplies.co.ke/sales/search/receipt', {
        receiptNumber, shopId
      }).then((response) => {
        this.searchLoading = false;
        this.receipt = response.data;
        this.receipt.date = new Date(this.receipt.date).toLocaleDateString()
        this.receipt.time = new Date(this.receipt.date).toLocaleTimeString()
        this.items = JSON.parse(this.receipt.saleItems);
      })
      .catch(err => {
        this.searchLoading = false;
        this.error = err.message
      })
    }
  }
});
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&family=Roboto+Mono&display=swap');
:root {
  --primary: #020402;
  --secondary: #FFE600;
  --disabled: #ECECEC;
  --fade: #9F9F9F;
  --bg: #F7F7F7;
  --grey: #858585;
}
body {
  font-family: 'Poppins', sans-serif;
  background: var(--bg)
}
.flexed {
  display: flex;
  gap: 1em;
}
.logo {
  margin-bottom: 2em;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 2em;
}
main {
  justify-content: space-between;
  gap: 3em;
}
.top-header {
  background: var(--primary);
  color: var(--secondary);
  padding: 0em 1.5em;
  border-radius: .75em;
  max-width: max-content;
  position: relative;
  &::after {
    content: '';
    position: absolute;
    right: -3em;
    top: -2.5em;
    display: inline;
    width: 100px;
    height: 100px;
    background: url('~@/assets/img/decorator.svg') no-repeat 50%;
  }
  #p-content {
    display: inline-block;
    margin-right: 2.5em;
  }
}

.search-form, .search-submit {
  margin: 2em 0;
}
.search-form {
  .flexed {
    gap: 3em;
    #shop-search {
      max-width: 180px;
    }
  }
}

.receipt {
  &-box {
    min-height: 70vh;
    width: 250px;
    position: relative;
    overflow: auto;
    &::-webkit-scrollbar {
      width: 5px;
    }

    /* Track */
    &::-webkit-scrollbar-track {
      box-shadow: inset 0 0 5px var(--fade); 
      border-radius: 10px;
    }
    
    /* Handle */
    &::-webkit-scrollbar-thumb {
      background: var(--grey);
      border-radius: 10px;
    }
    &.no-content {
      border: 3px dashed var(--fade);
      display: flex;
      justify-content: center;
      align-items: center;
      .inner {
        text-align: center;
        p {
          font-size: 70%;
          font-weight: 600;
          color: var(--fade)
        }
      }
    }
    &.with-content {
      padding: .5em;
      height: 100px;
    }
    &-card {
    position: relative;
    background: #fff;
    box-shadow: 0 0 1em rgba(0,0,0,0.2);
    padding: 2em 1em;
    max-width: 100%;
    height: auto;
    &-contents {
      font-size: 60%;
      font-family: 'Roboto Mono', monospace;
      .header {
        text-align: center;
        margin: 2em 0 1em;
      }
      p {
        margin: .45em 0;
      }
      .items {
        margin: 1em 0;
        &-head {
          font-weight: 600;
          border-bottom: 1px dashed var(--primary);
        }
        &-body {
          border-bottom: 1px dashed var(--primary);
          padding: .5em 0;
          margin-bottom: .5em;
        }
        &-footer {
          margin-bottom: 3em;
          .flexed {
            justify-content: space-between;
          }
        }
        &-bottom {
          position: absolute;
          bottom: 2em;
          font-size: 90%;
          .tiny {
            font-size: 80%;
          }
        }
      }
    }
  }
  }
  
}
.grid {
  display: grid;
  grid-template-columns: 100px 30px 50px auto;
  .last {
    text-align: right;
  }
}


</style>
