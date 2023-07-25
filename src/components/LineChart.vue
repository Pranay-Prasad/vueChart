<script>
import { Line } from 'vue-chartjs';
export default ({
    extends: Line,
    props:{
        // label:{type:String},
        chartData:{
            type: Array
        },
        options:{type:Object}
    },
    mounted(){
        const Maindata = this.chartData;
        this.renderChart({
            labels:['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
            datasets:[
                {
                    data:Maindata
                }
            ]
        },
        this.options
        )
    }
})
</script>