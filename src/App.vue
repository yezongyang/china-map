<style>
#map {
  width: 800px;
  height: 600px;
  margin: 0 auto;
}
</style>

<template>
  <div id="map"></div>
</template>

<script>
import * as d3 from 'd3';
import chinaJson from '@/assets/china-json/china.json';

export default {
  mounted() {
    this.projection = d3.geoMercator();
    this.path = d3.geoPath(this.projection);
    this.initMapScale(chinaJson);
    this.renderMap(chinaJson);
  },
  methods: {
    initMapScale(root) {
      this.projection.scale(1).translate([0, 0]).precision(0);
      const mapDom = document.querySelector('#map');
      const width = mapDom.clientWidth;
      const height = mapDom.clientHeight;
      const bounds = this.path.bounds(root);
      const scale = 0.95 / Math.max((bounds[1][0] - bounds[0][0]) / width, (bounds[1][1] - bounds[0][1]) / height);
      const t = [(width - scale * (bounds[1][0] + bounds[0][0])) / 2, (height - scale * (bounds[1][1] + bounds[0][1])) / 2];
      this.projection.scale(scale).translate(t);
    },
    renderMap(root) {
      const mapDom = document.querySelector('#map');
      const width = mapDom.clientWidth;
      const height = mapDom.clientHeight;

      const map = d3.select('#map')
        .append('svg')
        .attr('width', width)
        .attr('height', height)
        .append('g')
        .style('transform', 'translate(0,0)')
        .style('opacity', 0);

      map.transition().duration(400).style('opacity', 1);
      map.selectAll("path")
        .data(root.features)
        .enter()
        .append("path")
        .attr("class", "region")
        .attr("stroke", "#fff")
        .attr("stroke-width", 1)
        .attr("fill", function() {
            return "#5AB963";
        })
        .attr("d", this.path)
    }
  }
}
</script>
