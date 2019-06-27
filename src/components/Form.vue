<template>
    <div class="csv-upload container">
        <h1>CSV Line Chart Generator</h1>
        <form class="csv-upload__form" name="csv-upload" @submit="generateChart">
            <label class="csv-upload__field-label" for="csv-field">
                <span class="csv-upload__btn">Upload</span>
                <input ref="csvFile" type="file" @change="setSelectedFile" class="csv-upload__field-input" name="csv-file" id="csv-field" accept=".csv" />
            </label>
            <input type="submit" class="csv-upload__submit csv-upload__btn" value="Generate Chart" />
        </form>
        <p class="csv-upload__filename">{{filename}}</p>
        <div class="csv-upload__chart">
            <LineChart :chartData="dataSet" :options="options" />
        </div>
    </div>
</template>

<script>
import Papa from 'papaparse'
import VueCharts from 'vue-chartjs'
import { Bar, Line } from 'vue-chartjs'
import LineChart from './LineChart'

  export default {
    name: 'Form',
    components: {
        LineChart
    },
    data() {
        return {
            filename: 'No CSV file is selected',
            dataSet: {
                labels: [],
                datasets: []
            },
            options: {
                responsive: true, 
                maintainAspectRatio: false
            }
        }
    },
    methods: {
        setSelectedFile() {
            this.filename = this.$refs.csvFile.files[0].name
        },
        parseCSV (file) {
            var self = this
            self.dataSet.datasets = []
            var chartColors = ['#0293a4', '#f3af5a', '#c13f3d', '#158a44', '#999']

            Papa.parse(file, {
                error: function(err, file, inputElem, reason) {
                    alert('Please select CSV file')
                },
                complete: function(results) {
                    var result = results.data // Parsed data from the csv file

                    for(var i in result) {
                        var dataLabel = result[i][0], //Get series label
                            xValues = [],
                            yValues = [];
                        
                        result[i].shift() // Remove series label from array

                        // result[i] is an array of x and y value
                        for(var j in result[i]) {
                            var data = result[i][j].split('|') 
                            xValues.push(data[1]) // Separate x values to pass to dataset
                            yValues.push(data[0]) // Separate y values to pass to labels set
                        }

                        self.dataSet.labels = yValues.sort()

                        self.dataSet.datasets.push({
                            label: dataLabel,
                            fill: false,
                            borderColor: chartColors[i],
                            data: xValues
                        })   
                    }
                },
            })
        },
        generateChart(event) {
            event.preventDefault();

            var file = this.$refs.csvFile.files[0]
            if(file) {
                this.parseCSV(file)
            } else {
                alert('Please select CSV file')
            }
        }
    }
}
</script>