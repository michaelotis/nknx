<template>
            <v-card class='card-light'>
          <v-card-title>
            <span class="headline">{{ $t('message.openWallet') }}</span>
          </v-card-title>

          <v-card-text>
            <v-form v-model="valid" class="mb-4 w-100">
                <v-text-field 
                    :label="$t('message.nknWalletAddress')" 
                    v-model="addAddress"
                    required
                    :rules='addressRules'
                ></v-text-field>
                <v-text-field 
                    :label="$t('message.walletLabel')" 
                    v-model="label"
                ></v-text-field>
            </v-form>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" flat @click="resetModal">{{ $t('message.cancel') }}</v-btn>
            <v-btn color="blue darken-1" flat @click="addWallet">{{ $t('message.addWallet') }}</v-btn>
          </v-card-actions>
        </v-card>
</template>
<script>
  import axios from "axios";

  export default {
        props: {
            openWalletModalClosed: { type: Function }
        },
		mounted: function(){

		},
		methods: {
            resetModal(){
                    this.valid = false;
                    this.addAddress= "";
                    this.label = "";
                    this.openWalletModalClosed();
            },
			addWallet(){
                var self = this;
                axios.post('https://nknx.org:30003',{
					"jsonrpc": "2.0",
					"method":  "getunspendoutput",
					"params": {
						"address" : this.addAddress, 
						"assetid" : "4945ca009174097e6614d306b66e1f9cb1fce586cb857729be9e1c5cc04c9c02"
					},
					"id": 1
				}).then(function(response){
					if (response.data.error){
                        self.$store.dispatch("setSnackbar", self.$t('message.walletAddressError'));
                    }
                    else{
                        if(self.$auth.check()){
                            axios.post('walletAddresses/', {
                                'address':self.addAddress, 'label':self.label
                            })
                            .then((response) => {
                                self.$store.dispatch("addToWalletsStore", {"address": self.addAddress,"label":self.label});
                                self.$store.dispatch("setSelectedWallet", {"address": self.addAddress,"label":self.label});
                                self.$store.dispatch("setSnackbar", self.$t('message.walletAddressAdded'));
                                self.resetModal();
                            })
                            .catch((error) =>{
                                self.$store.dispatch("setSnackbar", self.$t('message.addressExistsError'));   
                            }) 
                        }
                        else{
                            self.$store.dispatch("addToWalletsStore", {"address": self.addAddress,"label":self.label});
                            self.$store.dispatch("setSelectedWallet", {"address": self.addAddress,"label":self.label});
                            self.$store.dispatch("setSnackbar", self.$t('message.walletAddressAdded'));
                            self.resetModal();
                        }
                    }
				})

			},
		},
		data () {
			return {
                label:"",
                valid:false,
                addAddress: "",
                addressRules: [
                v => !!v || "NKN Address is required!",
                v =>
                /(^N).*/.test(v) ||
                "NKN address must be valid"
                ],
			}
		}
  }
</script>