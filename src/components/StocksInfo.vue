<template>
  <div class="hello">
    <div class="container">
      <form class="navbar-form m-2">
        <div class="input-group no-border">
          <input type="text" value="" class="form-control" placeholder="Enter company stock name ..."
          v-model="stock">
          <button @click="update(stock)" class="btn btn-white btn-round bn-just-icon ml-3 search-button">
            <i class="material-icons">search</i>
          </button>
        </div> 
      </form>

    <div class="outerdiv" v-if="price  !== ''">
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
            <h4>Income Statement</h4>
            <div class="card-body table-responsive">
              <table class="table table-hover">
                <thead class="headingcustomtable">
                  <th>In Millions</th>
                  <th>{{ ISdate[0] }}</th>
                  <th>{{ ISdate[1] }}</th>
                  <th>{{ ISdate[2] }}</th>
                </thead>

                <tbody>
                  <tr>
                    <td>Revenue</td>
                    <td>${{ revenue[0] }}</td>
                    <td>${{ revenue[1] }}</td>
                    <td>${{ revenue[2] }}</td>
                  </tr>
                  <tr>
                    <td>COGS</td>
                    <td>${{ COGS[0] }}</td>
                    <td>${{ COGS[1] }}</td>
                    <td>${{ COGS[2] }}</td>
                  </tr>
                  <tr>
                    <td>Gross Profit</td>
                    <td>${{ GrossProfit[0] }}</td>
                    <td>${{ GrossProfit[1] }}</td>
                    <td>${{ GrossProfit[2] }}</td>
                  </tr>
                  <tr>
                    <td>Net Income</td>
                    <td>${{ Netinc[0] }}</td>
                    <td>${{ Netinc[1] }}</td>
                    <td>${{ Netinc[2] }}</td>
                  </tr>
                </tbody>

              </table>
          </div>

          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
          <button @click="showYearlyGraph(stock)" type="button" class="btn btn-primary">Analyze yearly price</button>
          <button @click="showBalanceSheetGraph(stock)" type="button" class="btn btn-primary ml-3">Analyze balance sheets</button>
          </div>
        </div>
        <div class="row mt-5">
          <div class="col-md-12">
            <div id="CandleStick"></div>
          </div>
        </div>
        <div class="row piegraphs">
            <div class="col-md-4">
              <div id="Assets"></div>
            </div>
            <div class="col-md-4">
              <div id="Liabilities"></div>
            </div>
            <div class="col-md-4">
              <div id="totalassets"></div>
            </div>

        </div>

      </div>
    </div>
  </div>
  </div>
</template>

<script>
import axios from 'axios'
import Plotly from 'plotly.js-dist';
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
      open: [],
      close: [],
      high: [],
      low: [],
      x: [],
      opentemp: '',
      xi: '',
      trace1: {},
      ISdate: [],
      revenue: [],
      COGS: [],
      GrossProfit: [],
      Opinc: [],
      Netinc: [],
      IStemp: '',
      show: false,
      show_pie: false,
      candle_data: [],
      candle_layout: {},
      again: '',

    }
  },
  methods: {
    update(stock){
      this.getInfo(stock);
      this.getCompanyValue(stock);
      this.getTable(stock);
      this.getChart(stock);
      this.getChartPie(stock);
    },
    showYearlyGraph(stock){
      this.show = !this.show;
      document.getElementById("CandleStick").style.display = (this.show == false)? "none" : "";
      this.getChart(stock);
    },
    showBalanceSheetGraph(stock){
      this.show_pie = !this.show_pie;
      document.getElementsByClassName("piegraphs")[0].style.display = (this.show_pie == false)? "none" : "";
      this.getChartPie(stock);
    },
    getTable(stock){
      this.revenue = [],
      this.COGS = [],
      this.GrossProfit = [],
      this.Opinc = [],
      this.Netinc = [],
      this.ISdate = [],
      this.IStemp = '',

      axios.get(`https://financialmodelingprep.com/api/v3/financials/income-statement/${stock}?period=quarter`)
      .then(res => {
        this.IStemp = res.data.financials
        this.IStemp = this.IStemp.slice(0,4)
        for(this.xi of this.IStemp){
          this.revenue.push(this.xi.Revenue/1000000)
          this.COGS.push(this.xi['Cost of Revenue']/1000000)
          this.GrossProfit.push(this.xi['Gross Profit']/1000000)
          this.Opinc.push(this.xi['Operating Income']/1000000)
          this.Netinc.push(this.xi['Net Income']/1000000)
          this.ISdate.push(this.xi['date'])
        }
      })
      .catch(err => console.log(err))
    },
    getChartPie(stock){
      axios.get(`https://financialmodelingprep.com/api/v3/financials/balance-sheet-statement/${stock}?period=quarter`)
      .then(res => {
        this.cash = res.data.financials[0]['Cash and cash equivalents']
        this.receivables = res.data.financials[0]['Receivables']
        this.inventories = res.data.financials[0]['Inventories']
        this.ppe = res.data.financials[0]['Property, Plant & Equipment Net']
        this.goodwill = res.data.financials[0]['Goodwill and Intangible Assets']
        this.LTInvestments = res.data.financials[0]['Long-term investments']
        this.Payables = res.data.financials[0]['Payables']
        this.STDebt = res.data.financials[0]['Short-term debt']
        this.LTDebt = res.data.financials[0]['Long-term debt']
        this.defRevenue = res.data.financials[0]['Deferred revenue']
        this.taxLiab = res.data.financials[0]['Tax Liabilities']
      })
      .catch(err => console.log(err))
      var data = [{
        values: [this.cash, this.receivables, this.inventories, this.ppe, this.goodwill, this.LTInvestments],
        labels: ['Cash', 'Receivables', 'Inventories', 'PPE', 'Goodwill', 'LT Inv.'],
        type: 'pie',
        textinfo: "label+percent",
        textposition: "outside",
        title: "Assets",
        automargin: true
        }];

        var layout = {
          height: 500,
          width: 400,
          showlegend: false
        };

        var dataliab = [{
          type: "pie",
          values: [this.Payables, this.STDebt, this.LTDebt, this.defRevenue, this.taxLiab],
          labels: ['Payables', 'Short-term debt', 'Long-term debt', 'Deferred revenue', 'Tax Liabilities'],
          type: 'pie',
          textinfo: "label+percent",
          textposition: "outside",
          title: "Liabilities",
          automargin: true
        }];

        var assetandliab = [{
          type: "pie",
          values: [this.cash, this.receivables, this.inventories, this.ppe, this.goodwill, this.LTInvestments,this.Payables, this.STDebt, this.LTDebt, this.defRevenue, this.taxLiab],
          labels: ['Cash', 'Receivables', 'Inventories', 'PPE', 'Goodwill', 'LT Inv.','Payables', 'Short-term debt', 'Long-term debt', 'Deferred revenue', 'Tax Liabilities'],
          type: 'pie',
          textinfo: "label+percent",
          textposition: "outside",
          title: "Total Assets and Liabilities",
          automargin: true
        }];
        
        if(document.getElementById("Assets") !== null){
          Plotly.newPlot('Assets', data, layout);
        }
        if(document.getElementById("Liabilities") !== null){
          Plotly.newPlot('Liabilities', dataliab, layout);
        }
        if(document.getElementById("totalassets") !== null){
          Plotly.newPlot('totalassets', assetandliab, layout);
        }
    },
    getChart(stock){
      axios.get("https://financialmodelingprep.com/api/v3/historical-price-full/"+ stock +"?timeseries=300")
      .then(res => {
        this.open = []
        this.close = []
        this.high = []
        this.low = []
        this.x = []
        this.opentemp = res.data.historical
        this.xi = ''
        for(this.xi of this.opentemp){
          this.open.push(this.xi.open)
          this.close.push(this.xi.close)
          this.high.push(this.xi.high)
          this.low.push(this.xi.low)
          this.x.push(this.xi.date)
        }
      })
      .catch(err => console.log(err))

        var trace1 = {
        
        x: this.x, 
        
        close: this.close, 
        
        decreasing: {line: {color: '#FF0000'}}, 
        
        high: this.high, 
        
        increasing: {line: {color: '#17BECF'}}, 
        
        line: {color: 'rgba(31,119,180,1)'}, 
        
        low: this.low, 
        
        open: this.open, 
        
        type: 'candlestick', 
        xaxis: 'x', 
        yaxis: 'y'
      };

      var data = [trace1];

      var layout = {
        dragmode: 'zoom', 
        margin: {
          r: 10, 
          t: 20, 
          b: 20, 
          l: 30
        }, 
        showlegend: false, 
        xaxis: {
          autorange: true, 
          rangeslider: {range: [this.x[0], this.x[1000]]}, 
          title: 'Date', 
          type: 'date'
        }, 
        yaxis: {
          autorange: true,
          range: [Math.min(...this.close)-10, Math.max(...this.close)+20],
          type: 'linear'
        },
      };
      this.candle_data = data;
      this.candle_layout = layout;
      if(document.getElementById("CandleStick") !== null &&this.show == true){
        Plotly.newPlot('CandleStick', data, layout);
      }
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
