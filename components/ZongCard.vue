<template>
<div>
    <v-row v-if="CounterResponse.ErrorMessage">
        <v-col cols="12">
           <v-card class="primary" dark>
                <v-list-item>
                <v-list-item-content>
                <div class="overline mb-4" >Error Message</div>
                <v-list-item-title class="headline mb-1">{{CounterResponse.ErrorMessage}}</v-list-item-title>
                </v-list-item-content>
                </v-list-item>
           </v-card>
        </v-col>
    </v-row>
    <v-row v-else>
        <v-col cols="3">
        <v-card class="secondary" dark>
        <v-list-item>
        <v-list-item-content>
        <div class="overline mb-4" >Total</div>
        <v-list-item-title class="headline mb-1">Balance</v-list-item-title>
        </v-list-item-content>

        <v-list-item-avatar
        size="80"
        class="grey--text"
        color="white"
        >
        {{CounterResponse.Total_Balance}}
        </v-list-item-avatar>

        </v-list-item>

        </v-card>
        </v-col>
        <v-col cols="3">
             <v-card class="secondary" dark>
            <v-list-item class="secondary" dark>
            <v-list-item-content>
            <div class="overline mb-4" >Total</div>
            <v-list-item-title class="headline mb-1">Broadcasted</v-list-item-title>
            </v-list-item-content>

            <v-list-item-avatar
            size="80"
            class="grey--text"
            color="white"
            >
            {{CounterResponse.Total_Broadcasted}}
            </v-list-item-avatar>
            </v-list-item>
             </v-card>
        </v-col>
        <v-col cols="3">
             <v-card class="secondary" dark>
            <v-list-item class="secondary" dark>
            <v-list-item-content>
            <div class="overline mb-4" >Total</div>
            <v-list-item-title class="headline mb-1">Successful</v-list-item-title>
            </v-list-item-content>

            <v-list-item-avatar
            size="80"
            class="grey--text"
            color="white"
            >
            {{CounterResponse.Total_Successful}}
            </v-list-item-avatar>
            </v-list-item>
             </v-card>
        </v-col>
        <v-col cols="3">
             <v-card class="secondary" dark>
            <v-list-item class="secondary" dark>
            <v-list-item-content>
            <div class="overline mb-4" >Total</div>
            <v-list-item-title class="headline mb-1">Failed</v-list-item-title>
            </v-list-item-content>

            <v-list-item-avatar
            size="80"
            class="grey--text"
            color="white"
            >
            {{CounterResponse.Total_Failed}}
            </v-list-item-avatar>
            </v-list-item>
             </v-card>
        </v-col>
    </v-row>
</div> 
</template>



<script>

export default {


data: () => ({
CounterResponse: {
Total_Balance: '',
Total_Broadcasted: '',
Total_Successful: '',
Total_Failed: '',
User_Masks: "",
ErrorMessage: null
}
}),
async created () {  
      
   await this.$axios.get('totalbalance')
   .then(res => {

     Object.assign(this.CounterResponse,res.data.balance)
     console.log(res.data);

   })
   .catch(err => console.log(this.CounterResponse.ErrorMessage = err))  
},
methods : {

},
 
}

</script>
