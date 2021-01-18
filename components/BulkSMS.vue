<template>
<div>
   <v-snackbar  v-model="snackbar" :top="'top'">{{msg}}</v-snackbar>
<v-form ref="form">
                    <v-file-input
                    v-model="editedItem.file"
                    accept=".csv"
                    show-size
                    label="CSV"
                    ></v-file-input>
                  
                <v-autocomplete
                   v-model="editedItem.mask_name"
                   :items="masks"
                   item-text="mask_name"
                   item-value="id"
                   label="Maskings"
                   >
                   </v-autocomplete> 

                    <v-autocomplete
                   v-model="editedItem.campaign"
                   :items="campaigns"
                   item-text="campaign_name"
                   item-value="id"
                   label="Campaigns"
                   >
                   </v-autocomplete> 

                 <v-textarea @keyup="get_counter" counter :rules="rules4msg" v-model="editedItem.msg" label="Message"></v-textarea>
                  Total Messages will be consumed : {{counter}}

                  <br>
                   
                <v-btn class="secondary" text @click="save">Save</v-btn>
               </v-form>
          
  </div>

</template>

<script>
  export default {
    data: () => ({
      snackbar : false,
      msg : '',
      dialog: false,
      search:'',
      headers: [
        {
          text: 'id',
          sortable: true,
          value: 'id',
        },
       
        { text: 'Actions', value: 'actions', sortable: false },

      ],
      data: [],
      cap : 160,
      counter : 1,
      capUpdater : 1,
      editedIndex: -1,
      editedItem: {
       mask_name : '',   
       file: [],
       number:'',
       campaign:'',
       msg : '',
      },
      defaultItem: {
       mask_name : '',
       file: [],
       number:'',
       campaign:'',
       msg : '',
      },
      campaigns : [],
      masks : [], 
      rules4number : [
          v => /^92/.test(v) || 'Must start with 92 instead of 0',
          v => /^\d+$/.test(v) || 'Must be a number',
          v => v.length < 13 || 'Must be less than 13 character'
      ],
      rules4msg : [ 
          v => !!v || 'This field is required',
      ],
      CampaignRules : [],
     
    }),



    computed: {

      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
     
    },

    watch: {
      dialog (val) {
        val || this.close()
      },      
    },

    created () {
     
      this.getCampaign()
      this.getMasks()
    },

    methods: {

      getCampaign(){
        this.$axios.get('campaign').then(res => this.campaigns = res.data);
      },
      getMasks(){
        this.$axios.get('mask').then(res => this.masks = res.data);
      },

      get_counter () {
        
        let len = this.editedItem.msg.length;
        var nextCap = this.cap * this.counter;
        var prevCap = this.cap * (this.counter - 1);

        if(len == 0) this.counter = 1
        
        if(len > nextCap) this.counter++

        if(len <= prevCap) this.counter-- 

      },



      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {

          // const bs = new FormData();

          // bs.append('attachment',this.editedItem.file);
          // bs.append('campaign',this.editedItem.campaign);
          // bs.append('msg',this.editedItem.msg);


          var payload  = {
            campaign_id : this.editedItem.campaign,
            message : this.editedItem.msg
          }

          
            if(this.$refs.form.validate()){
              this.$axios.post('bulk_sms',payload)
              .then((res) => {

                this.snackbar = true;
                this.msg = 'Message Submitted Successfully';
              
                console.log(res.data);
                this.close()
              
            
            });
            }
     
      },
    },
  }
</script>