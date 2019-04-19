<template>
    <l-tooltip v-observe-visibility="{callback: getPredictions, throttle: 300}">
        <div>
            <ul>
                <li>{{stop.display_name}}</li>
                <li v-if="predictions !== null">{{predictions.items[0].minutes}} minutes to next bus/train</li>
            </ul>
        </div>
    </l-tooltip>
</template>

<script>
export default {
    props: [
        "stop",
        "routeId"
    ],
    data() {
        return {
            predictions: null,
        }
    },
    methods: 
    {
        async getPredictions(isVisible)
        {
            if(isVisible)
            {
                console.log(this.$props.routeId)
                if(this.$props.routeId.includes("8"))
                    this.predictions = await this.$axios.$get(`http://api.metro.net/agencies/lametro-rail/stops/${this.stop.id}/predictions/`)
                else
                    this.predictions = await this.$axios.$get(`http://api.metro.net/agencies/lametro/stops/${this.stop.id}/predictions/`)
            }
        },
        succ(isVisible)
        {
            
            console.log("succ")
        }
    }
}
</script>

<style>

</style>
