<template>
	<div>
	
		<page-title-bar></page-title-bar>
		<v-container grid-list-xl pt-0>
			<app-section-loader :status="loader"></app-section-loader>
			<v-layout row wrap>
				<app-card
					customClasses="mb-30"
					colClasses="xl12 lg12 md12 sm12 xs12"
					:heading="$t('message.map')"
				>
					<v-layout row wrap>
						<v-flex xs12>
							<map-widget :query="query"></map-widget>
						</v-flex>
					</v-layout>
				</app-card>
				<app-card
					customClasses="mb-30"
					colClasses="xl6 lg6 md12 sm12 xs12"
				>
					<v-layout row wrap>
						<v-flex xs12>
							<NetworkCountries-chart></NetworkCountries-chart>
						</v-flex>
					</v-layout>
				</app-card>
				<app-card
					customClasses="mb-30"
					colClasses="xl6 lg6 md12 sm12 xs12"
				>
					<v-layout row wrap>
						<v-flex xs12>
							<NetworkProviders-chart></NetworkProviders-chart>
						</v-flex>
					</v-layout>
				</app-card>
				<app-card
					customClasses="mb-30"
					colClasses="xl12 lg12 md12 sm12 xs12"
				>
					<v-layout row wrap>
						<v-flex xs12>
							<NetworkBlocks-chart></NetworkBlocks-chart>
						</v-flex>
					</v-layout>
				</app-card>
				<app-card
					customClasses="mb-30"
					colClasses="xl12 lg12 md12 sm12 xs12"
				>
					<v-layout row wrap>
						<v-flex xs12>
							<NetworkTransactions-chart></NetworkTransactions-chart>
						</v-flex>
					</v-layout>

				</app-card>
				<app-card
					customClasses="mb-30"
					colClasses="xl12 lg12 md12 sm12 xs12"
				>
				<v-layout row wrap>
						<v-flex xs12>
							<NetworkTransfers-chart></NetworkTransfers-chart>
						</v-flex>
					</v-layout>

				</app-card>


			</v-layout>
		</v-container>
	
    </div>
</template>
<script>

import axios from "axios";
import MapWidget from "Components/Widgets/Map";
import NetworkCountriesChart from "Components/Widgets/NetworkCountriesChart";
import NetworkProvidersChart from "Components/Widgets/NetworkProvidersChart";
import NetworkBlocksChart from "Components/Widgets/NetworkBlocksChart";
import NetworkTransactionsChart from "Components/Widgets/NetworkTransactionsChart";
import NetworkTransfersChart from "Components/Widgets/NetworkTransfersChart";

export default {
   components: {
	MapWidget,
	NetworkCountriesChart,
	NetworkProvidersChart,
	NetworkBlocksChart,
	NetworkTransactionsChart,
	NetworkTransfersChart,
  },
  data() {
    return {
		loader: true,
		query: "?latest=1&txType=66"
    };
  },
	mounted: function(){
		this.loader = false
	},

  methods: {

  }
};
</script>
