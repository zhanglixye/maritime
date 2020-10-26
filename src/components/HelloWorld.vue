<template>
    <div id="container">
        {{msg}}
    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        props: {
            msg: String
        },
        created() {
            //console.log($);
        },
        mounted() {
          this.test();
        },
        methods: {
            test() {
                let dom = document.getElementById('container');
                let myChart = this.$echarts.init(dom);
                //let app = {};
                let option = null;
                myChart.showLoading();
                let graph = require('../assets/hecore');
                myChart.hideLoading();
                var categories = [];
                for (var i = 0; i < 9; i++) {
                    categories[i] = {
                        name: '类目' + i
                    };
                }
                graph.nodes.forEach(function (node) {
                    node.itemStyle = null;
                    node.value = node.symbolSize;
                    node.symbolSize /= 1.5;
                    node.label = {
                        show: node.symbolSize > 30
                    };
                    node.category = node.attributes.modularity_class;
                });
                option = {
                    title: {
                        text: 'Les Miserables',
                        subtext: 'Default layout',
                        top: 'bottom',
                        left: 'right'
                    },
                    tooltip: {},
                    legend: [{
                        // selectedMode: 'single',
                        data: categories.map(function (a) {
                            return a.name;
                        })
                    }],
                    animationDuration: 1500,
                    animationEasingUpdate: 'quinticInOut',
                    series: [
                        {
                            name: 'Les Miserables',
                            type: 'graph',
                            layout: 'none',
                            data: graph.nodes,
                            links: graph.links,
                            categories: categories,
                            roam: true,
                            focusNodeAdjacency: true,
                            itemStyle: {
                                borderColor: '#fff',
                                borderWidth: 1,
                                shadowBlur: 10,
                                shadowColor: 'rgba(0, 0, 0, 0.3)'
                            },
                            label: {
                                position: 'right',
                                formatter: '{b}'
                            },
                            lineStyle: {
                                color: 'source',
                                curveness: 0.3
                            },
                            emphasis: {
                                lineStyle: {
                                    width: 10
                                }
                            }
                        }
                    ]
                };
                myChart.setOption(option);
                if (option && typeof option === "object") {
                    myChart.setOption(option, true);
                }
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    #container {
        width: 1000px;
        height: 1000px;
    }
</style>
