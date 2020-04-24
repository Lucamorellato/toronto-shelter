<template>
  <div class="single-chart">
    <svg viewBox="0 0 36 36" class="circular-chart" :class="capacityColor" >
      <path class="circle-bg"
        d="M18 2.0845
          a 15.9155 15.9155 0 0 1 0 31.831
          a 15.9155 15.9155 0 0 1 0 -31.831"
      />
      <path class="circle"
        :stroke-dasharray="`${parseInt(occupiedPercentage)}, 100`"
        d="M18 2.0845
          a 15.9155 15.9155 0 0 1 0 31.831
          a 15.9155 15.9155 0 0 1 0 -31.831"
      />
      <text x="18" y="20.35" class="percentage"><span class="visuallyhidden">{{sector}} capacity at</span>{{occupiedPercentage}}</text>
    </svg>
  </div>
</template>

<script>
export default {
    props: {
        sector: String,
        occupiedPercentage: String,
        capacityColor: String,
    },
}
</script>

<style>
/* circle chart */
.single-chart {
  justify-content: space-around ;
}

.circular-chart {
  display: block;
  margin: 10px auto;
  max-width: 80%;
  max-height: 250px;
  min-width: 225px;
}

.circle-bg {
  fill: none;
  stroke: #eee;
  stroke-width: 3;
}

.circle {
  fill: none;
  stroke-width: 3.6;
  stroke-linecap: round;
  animation: progress 1s cubic-bezier(.4,.4,.25,1) forwards;
}

@keyframes progress {
  0% {
    stroke-dasharray: 0 100;
  }
}

.circular-chart.red .circle {
  stroke: rgb(245, 70, 20);
}
.circular-chart.red .circle-bg {
  fill: rgba(245, 70, 20, 0.2);
}
.circular-chart.orange .circle {
  stroke: rgb(245, 146, 25);
}
.circular-chart.orange .circle-bg {
  fill: rgba(245, 146, 25, 0.2);
}
.circular-chart.yellow .circle {
  stroke: rgb(250, 230, 0);
}
.circular-chart.yellow .circle-bg {
  fill: rgba(250, 230, 0, 0.2);
}

.circular-chart.normal .circle {
  stroke: rgb(41, 204, 84);
}
.circular-chart.normal .circle-bg {
  fill: rgba(41, 204, 84, 0.2);
}

.percentage {
  fill: #274490;
  font-family: sans-serif;
  font-size: 0.4em;
  font-weight:900;
  text-anchor: middle;
}

.circular-chart {
  display: block;
  max-height: 250px;
}
/* circle chart */
</style>