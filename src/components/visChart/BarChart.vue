<template>
  <svg class="barChart">

  </svg>
</template>

<script>
  import * as d3 from 'd3'

  export default {
    name: 'barchart',
    props:['regionQueryData', 'svFeatures2Color'],
    data () {
      return {
        title: 'BarChart',
      }
    },
    mounted(){
//      console.log('this', this.regionQueryData, this.svFeatures2Color)
      this.drawBarChart();
    },
    computed:{

    },
    methods:{
      drawBarChart(){
        let _this = this;
        let width = this.$el.clientWidth;
        let height = this.$el.clientHeight;
        let featureArr = this.svFeatures2Color['allFeatures'];
        let barNum = featureArr.length;
        let featureTextLength = 60;
        console.log('width', width, height, this.svFeatures2Color);

        let svgContainer = d3.select(this.$el).append('g')
        let barsContainer = svgContainer.selectAll('.bar')
          .data(featureArr)
          .enter()
          .append('g')
          .attr('class','bar')
          .attr('transform', function(d, i){
            return 'translate(' + 0 + ',' + (i * height / (barNum + 1)) + ')';
          })
        let bars = barsContainer.append('rect')
          .attr('x', featureTextLength)
          .attr('width', function(d){
            return _this.regionQueryData['featureRatio'][d] * width * 1.5;
          })
          .attr('height', function(d){return height / (barNum + 1) - 10})
          .attr('fill', function(d){return _this.svFeatures2Color[d]})
          .attr('fill-opacity', 0.8)
        bars.append('title').text(function(d){return d});

        barsContainer.append('text').text(function(d){return d})
          .attr('x', 5)
          .attr('y', 10)
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .barChart{
    margin-top: 20px;
    width: 100%;
    height: 120px
  }
</style>
