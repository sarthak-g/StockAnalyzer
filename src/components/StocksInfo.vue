<template>
  <div class="hello">
    <div class="container">
      <form class="navbar-form m-2">
        <div class="input-group no-border">
          <input type="text" value="" class="form-control" placeholder="Search..."
          v-model="stock">
          <button @click="update(stock)" class="btn btn-white btn-round bn-just-icon ml-3">
            <i class="material-icons">search</i>
          </button>
        </div> 
      </form>

      <div class="row">
        <div class="col-md-3">
          <div class="card card-stats card-background_first">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">content_copy</i>
              </div>
              <p class="card-category">Beta</p>
              <h4 class="card-title">{{ beta }}</h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="card card-stats card-background_second">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">store</i>
              </div>
              <p class="card-category">CEO</p>
              <h4 class="card-title">{{ CEO }}</h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="card card-stats card-background_third">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">info</i>
              </div>
              <p class="card-category">Company name</p>
              <h4 class="card-title">{{ companyName }}</h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="card card-stats card-background_fourth">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">storage</i>
              </div>
              <p class="card-category">Sector</p>
              <h4 class="card-title">{{ sector }}</h4>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          <md-list>

            <md-list-item class="col-md-3">
              <md-icon>timeline</md-icon>
              <span class="left">{{ price }}</span>
              <span class="right">P/B {{ priceBookValueRatio }}</span>
            </md-list-item>

            <md-list-item class="col-md-3 ">
              <md-icon>attach_money</md-icon>
              <span class="left">ROA {{ROA}}%</span>
              <span class="right">P/S {{ pricetosales }}</span>
            </md-list-item>

            <md-list-item class="col-md-3">
              <md-icon>attach_money</md-icon>
              <span class="left">ROE {{ ROE }}%</span>
              <span class="right">P/E {{ priceEarnings }}</span>
            </md-list-item>

            <md-list-item class="col-md-3">
              <md-icon>bar_chart</md-icon>
              <span class="">Gross Profit {{ grossProfitMargin }}%</span>
            </md-list-item>

          </md-list>
        </div>

        <div class="row mt-5">
          <div class="col-md-4">
            <div class="card card-profile border-0">
              <div class="card-avatar">
                <img :src="image" alt="" class="img">
              </div>
              <div class="card-body">
                <h4 class="card-title no-outline">{{ companyName }}</h4>
                <p class="card-description">{{ description }}</p>
              </div>
            </div>
          </div>
          <div class="col-md-8">
            PLACEHOLDER FOR A CHART
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'StocksInfo',
  props: {
    msg: String
  },
  data(){
    return {
      stock: '',
      beta: '',
      infoCompany: '',
      CEO: '',
      sector: '',
      companyName: '',
      price: '',
      marketCap: '',
      image: '',
      description: '',
      ROA: '',
      ROE: '',
      priceBookValueRatio: '',
      grossProfitMargin: '',
      pricetosales: '',
      priceEarnings: '',

    }
  },
  methods: {
    update(stock){
      this.getInfo(stock);
      this.getCompanyValue(stock);
    },
    getCompanyValue(stock){
      axios.get("https://financialmodelingprep.com/api/v3/financial-ratios/" + stock)
      .then(res => {
        this.ROA = parseFloat(res.data.ratios[0]
        .profitabilityIndicatorRatios.returnOnAssets*100).toFixed(2);
        this.ROE = parseFloat(res.data.ratios[0]
        .profitabilityIndicatorRatios.returnOnEquity*100).toFixed(2);
        this.grossProfitMargin = parseFloat(res.data.ratios[0]
        .profitabilityIndicatorRatios.grossProfitMargin*100).toFixed(2);
        this.priceBookValueRatio = parseFloat(res.data.ratios[0]
        .investmentValuationRatios.priceBookValueRatio).toFixed(2);
        this.pricetosales = parseFloat(res.data.ratios[0]
        .investmentValuationRatios.priceToSalesRatio).toFixed(2);
        this.priceEarnings = parseFloat(res.data.ratios[0]
        .investmentValuationRatios.priceEarningsRatio).toFixed(2);
      })
      .catch(err => console.log(err))
    },
    getInfo(stock) {
      axios.get("https://financialmodelingprep.com/api/v3/company/profile/" + stock)
      .then(res => {
        this.infoCompany = res.data.profile
        this.beta = parseFloat(this.infoCompany.beta).toFixed(2);
        this.CEO = this.infoCompany.ceo;
        this.sector = this.infoCompany.sector;
        this.companyName = this.infoCompany.companyName;
        this.price = this.infoCompany.price;
        this.marketCap = this.infoCompany.mktCap,
        this.image = this.infoCompany.image,
        this.description = this.infoCompany.description
      })
      .catch(err => console.log(err))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.card-background_first{
  background-color: lightseagreen;
  color: white;
}
.card-background_second{
  background-color: darkgreen;
  color: white;
}
.card-background_third{
  background-color: gold;
  color: white;
}
.card-background_fourth{
  background-color: rebeccapurple;
  color: white;
}

.md-list{
  max-width: 100%;
  display: inline-block;
  vertical-align: top;
  border: 1px solid rgba(#000, .12);
}

.left{
  margin-right: 12px;
  margin-top: 0px;
}

.right{
  margin-left: 5px;
  margin-top: 0px;
}
</style>
