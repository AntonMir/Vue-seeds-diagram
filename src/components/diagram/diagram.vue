<template>
    <figure class="highcharts-figure">
        <div id="container"></div>
    </figure>
</template>

<script>
const Highcharts = require('highcharts');
export default {
    props: [
        'currentSeeds'
    ],
    data() {
        return {
            // конфиги графика
            setOptions: {
                colors: [
                    '#42CBFD', '#B4E6CA', '#969AB2', 
                    '#DDDF00', '#24CBE5', '#64E572', 
                    '#FF9655', '#FFF263', '#6AF9C4', 
                    '#004634', '#2ab18f', '#281096',
                    '#80110d', '#c319c9', '#82b12c'
                ]
            },
            config: {
                chart: {
                    type: 'area'
                },
                title: {
                    text: 'Урожайность семян на Луне'
                },
                subtitle: {
                    text: 'по мотивам книги "Незнайка на луне"'
                },
                xAxis: {
                    categories: ['2025', '2026', '2027', '2028'],
                    title: {
                        text: 'годы',
                        enabled: true
                    }
                },
                yAxis: {
                    title: {
                        text: 'шт./м2',
                    }
                },
                tooltip: {
                    pointFormat: `
                        <span style="color:{series.color}">
                            {series.name}
                        </span>: <b>{point.percentage:.1f}</b> ({point.y:,.0f})<br/>`,
                    split: true
                },
                plotOptions: {
                    area: {
                        lineColor: '#7B8B90',
                        lineWidth: 1,
                        marker: {
                            lineWidth: 1,
                            lineColor: '#7B8B90'
                        }
                    }
                },
                series: this.currentSeeds,
            }
        }
    },
    mounted() {
        this.render();
    },
    watch: {
        config: {
            deep: true,
            handler() {
                this.render()
            }
        },
        currentSeeds: {
             handler() {
                // если массив, переданный из родительского компонента изменится, 
                // перезаписать значение в переменную
                this.config.series = this.currentSeeds
            }
        }
    },
    methods: {
        render() {
            // переотрисовка, если изменился конфиг              
            Highcharts.chart('container', {...this.config});
            Highcharts.setOptions({colors: this.setOptions.colors});
        }
    },
}

</script>

<style lang="scss" src="./diagram.sass"></style>
