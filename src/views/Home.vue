<template>
  <div >
    <v-container class="grey lighten-2" >
      
        <v-row class="mt-12 mb-12 " justify="space-between" align="center">
         
            <v-col cols="12" md="6">
              <v-img
                lazy-src="https://picsum.photos/id/11/10/6"
                aspect-ratio="2"
                max-height="150"
                contain
                src="https://pngimg.com/uploads/beef/beef_PNG45.png"
              >
              </v-img>
            </v-col>
            <v-col cols="12" md="6">
              <div class="ml-5">
                <div class="text-h2  text-capitalize red--text text--darken-1 font-weight-regular">
                  meat price
                </div>
                <div class="grey--text">Meat price in world countries</div>
              </div>
            </v-col>
        
        </v-row>
     
      <div class="mt-12">
        <v-row justify="space-around" align="center" class="mt-16" >
          <v-col cols="12" md="4">
            <v-autocomplete :items="countryNames" 
                item-text="country_name"
                item-value="country_code"
                return-object 
                v-model="contry" 
                label="Countries"
                max-width="150"
                  outlined 
                  background-color="white">
            </v-autocomplete>
            <v-btn class=" red lighten-2" depressed dark large block max-width="300" @click="showResult">
              show Result
            </v-btn>
          </v-col>
        
          <v-col cols="12" md="4" v-if="breaker">
            <v-card flat class="pa-5" color="#EEEEEE">
              <v-row>
                <v-col>
                <v-card-title >
              <v-row align="center" justify="space-around">
                <v-col cols="12" >
                  <div class="d-flex justify-center align-center">
                  <v-img
                    :src="contryImgeSrc"
                    max-height="40"
                    max-width="40"
                  >
                  </v-img>
                  <span class="title font-weight-light ml-2 blue-grey--text text--lighten-2">{{ mathingVmodelWithButton.country_name }} </span>
                  </div>
                </v-col>
              </v-row>
                </v-card-title>
              <!-- <v-card-actions> -->
              <v-row justify="center" align="center" v-if="breaker==true" class="mt-5">
                <v-col cols="6" >
                  <div class="d-flex justify-center align-center">
                    <v-avatar>
                      <v-img
                        class="elevation-6"
                        alt=""
                        src="./../assets/mollon.jpg"
                      ></v-img>
                    </v-avatar>
                    <span class="ml-1 blue-grey--text text--lighten-2">{{ Math.round(mollonPrice) }}</span>
                    <span class="ml-1 text-caption">{{mathingVmodelWithButton.currency_code}}</span>
                  </div>
                </v-col>
              
              
                <v-col cols="6">
                  <div class="d-flex justify-center align-center">
                    <v-avatar >
                      <v-img
                        class="elevation-6"
                        src="./../assets/bovine.jpg"
                      ></v-img>
                    </v-avatar>
                    <span class="ml-2 blue-grey--text text--lighten-2">{{ Math.round(calculateBovinePrice) }}</span>
                    <span class="ml-1 text-caption">{{mathingVmodelWithButton.currency_code}}</span>
                  </div>
                 
                </v-col>
              </v-row>
              <!-- </v-card-actions> -->
              </v-col>
              </v-row>
            </v-card>
          </v-col>
          <div v-else>
            <v-progress-circular indeterminate color="primary">
            </v-progress-circular>
          </div>
        </v-row>
      </div>
    </v-container>
  </div>
</template>

<script>
import countryData from "../../data/country.json"
export default {
  name: "Home",
  data() {
    return {
      contry:{
        country_name:"Sudan",
        country_code:"SD",
        currency_code:"SDG",
        currency_name:"SDG"

      },
      moneyRate:"",
      countryNames: countryData,
      baseMollonPrice:4.32,
      baseBovinePrice:4,
      breaker:false,
      mathingVmodelWithButton:''
    };
  },
  methods:{
  
    async getRates(){
      this.breaker = false
      const res = await fetch(' http://api.exchangeratesapi.io/v1/latest?access_key=e540e1acdd4f73a85dcf93107bebd2bd');
      const data = await res.json();
      this.moneyRate = data;
      this.breaker = true;

    },
    showResult(){
      this.mathingVmodelWithButton = this.contry
    }
  },
  components: {
    },
  mounted () {
       this.getRates();
       this.mathingVmodelWithButton = this.contry
  },
  computed: {
    contryImgeSrc(){
      return `https://www.countryflags.io/${this.mathingVmodelWithButton.country_code}/shiny/64.png`
  },
    currencyCode(){
      return this.mathingVmodelWithButton.currency_code
    },
    mollonPrice(){
      return this.baseMollonPrice * this.moneyRate.rates[this.currencyCode]
    },
    calculateBovinePrice(){
      return this.baseBovinePrice * this.moneyRate.rates[this.currencyCode]
    }
    
      },
};
</script>
