<template>
    <div class="table-responsive">
        <app-section-loader :status="loader"></app-section-loader>
        <v-card-title><h3>{{$t('message.minersTotal')}}: {{totalMiners}}</h3></v-card-title>
        <MinersChart style='height:300px'></MinersChart>
        <v-card-title><i>{{$t('message.topMinersDecr')}}</i></v-card-title>
    </div>
</template>
<script>
import axios from "axios";
import MinersChart from "Components/Charts/MinersChart";
import { Timeouts } from "Constants/timeouts";

export default {
    components: {
        MinersChart
    },
    data() {
        return {
            interval: null,
            loader: true,
            totalMiners: 'Loading...'
        };
    },
    destroyed() {
        clearInterval(this.interval);
    },
    created: function() {
        this.getMiners()
    },
    mounted: function() {
        this.interval = setInterval(this.getMiners, Timeouts.medium);
    },
    methods: {
        getMiners(){
            const self = this;
            self.loader = true
            axios.get('statistics/miners').then(function(addresses){
                self.totalMiners = addresses.data.length
            });
            self.loader = false
        }
    }
};
</script>