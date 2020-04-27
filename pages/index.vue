<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm12
      md12
    >
      <div id="myChart" />
    </v-flex>
  </v-layout>
</template>

<script>
// import echarts from 'echarts'
// eslint-disable-next-line no-unused-vars
if (process.client) {
  require('echarts-wordcloud')
}
export default {
  components: {
  },
  async asyncData (context) {
    try {
      const res = await context.$axios.get('http://api.tianapi.com/txapi/weibohot/index?key=6a0402fa89a9122b27cc47ca462376f1')
      console.info('res====>', res)
      const words = res.data.newslist.map((n) => {
        return {
          name: n.hotword,
          value: n.hotwordnum
        }
      })
      console.info('words---->', words)
      return {
        words
      }
    } catch (e) {
      console.error('error getting words', e)
    }
  },
  mounted () {
    this.genWordCloud()
    // setTimeout(() => {
    //   console.info('this.words--->', this.words)
    // }, 222)
  },
  methods: {
    genWordCloud () {
      const chart = this.$echarts.init(document.getElementById('myChart'))
      // const chart = echarts.init(document.getElementById('myChart'))
      chart.setOption({
        series: [{
          type: 'wordCloud',

          // The shape of the "cloud" to draw. Can be any polar equation represented as a
          // callback function, or a keyword present. Available presents are circle (default),
          // cardioid (apple or heart shape curve, the most known polar equation), diamond (
          // alias of square), triangle-forward, triangle, (alias of triangle-upright, pentagon, and star.

          shape: 'circle',

          // A silhouette image which the white area will be excluded from drawing texts.
          // The shape option will continue to apply as the shape of the cloud to grow.

          // maskImage,

          // Folllowing left/top/width/height/right/bottom are used for positioning the word cloud
          // Default to be put in the center and has 75% x 80% size.

          left: 'center',
          top: 'center',
          width: '70%',
          height: '80%',
          right: null,
          bottom: null,

          // Text size range which the value in data will be mapped to.
          // Default to have minimum 12px and maximum 60px size.

          sizeRange: [12, 60],

          // Text rotation range and step in degree. Text will be rotated randomly in range [-90, 90] by rotationStep 45

          rotationRange: [-90, 90],
          rotationStep: 45,

          // size of the grid in pixels for marking the availability of the canvas
          // the larger the grid size, the bigger the gap between words.

          gridSize: 8,

          // set to true to allow word being draw partly outside of the canvas.
          // Allow word bigger than the size of the canvas to be drawn
          drawOutOfBound: false,

          // Global text style
          textStyle: {
            normal: {
              fontFamily: 'sans-serif',
              fontWeight: 'bold',
              // Color can be a callback function or a color string
              color () {
              // Random color
                return 'rgb(' + [
                  Math.round(Math.random() * 160),
                  Math.round(Math.random() * 160),
                  Math.round(Math.random() * 160)
                ].join(',') + ')'
              }
            },
            emphasis: {
              shadowBlur: 10,
              shadowColor: '#333'
            }
          },

          // Data is an array. Each array item must have name and value property.
          //   data: [{
          //     name: 'Farrah Abraham',
          //     value: 1,
          //     // Style of single text
          //     textStyle: {
          //       normal: {},
          //       emphasis: {}
          //     }
          //   },
          //   {
          //     name: '22222222222222222',
          //     value: 2,
          //     // Style of single text
          //     textStyle: {
          //       normal: {},
          //       emphasis: {}
          //     }
          //   },
          //
          //   {
          //     name: '111111',
          //     value: 3,
          //     // Style of single text
          //     textStyle: {
          //       normal: {},
          //       emphasis: {}
          //     }
          //   },
          //   {
          //     name: '44444',
          //     value: 4
          //   },
          //   {
          //     name: '55555',
          //     value: 4
          //   }
          //
          //   ]
          data: this.words
        }]
      })
    //   const
    //     option = {
    //       xAxis: {
    //         type: 'category',
    //         data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
    //       },
    //       yAxis: {
    //         type: 'value'
    //       },
    //       series: [{
    //         data: [820, 932, 901, 934, 1290, 1330, 1320],
    //         type: 'line'
    //       }]
    //     }
    //   chart.setOption(option)
    //   console.log('chart----->', chart)
    }
  }
}
</script>
<style scoped>
  #myChart{
    width: calc(100vw - 10%);
    height: calc(100vh - 150px);
    margin-left: auto;
    margin-right: auto;
    float: left;
  }
</style>
