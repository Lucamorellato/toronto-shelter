<template>
    <div class="top-graph">
        <p class="value"><span class="visuallyhidden">{{sector}} capacity at</span>{{occupiedPercentage}}</p>
        <div class="circle-container">
            <svg viewBox="0 0 36 36" class="circular-chart" :stroke="`rgb(${capacityColor})`" :fill="`rgba(${capacityColor}, 0.2)`" >
                <path class="circle" :stroke-dasharray="`${parseInt(occupiedPercentage)}, 100`" d="M18 2.0845
                a 15.9155 15.9155 0 0 1 0 31.831
                a 15.9155 15.9155 0 0 1 0 -31.831"
            />
            </svg>
        </div>
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
.top-graph {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.top-graph .value {
  position: absolute;
}

.value {
  font-size: 30px;
  font-weight: bold;
}

.circular-chart {
  display: block;
  max-height: 250px;
}

.circle-container {
    background: #e8e7e7;
    border-radius: 50%;
}

.circle {
  stroke-width: 2;
  stroke-linecap: round;
  animation: progress 1s cubic-bezier(.4,.4,.25,1) forwards;
  padding: 5px;
}

@keyframes progress {
  0% {
    stroke-dasharray: 0 100;
  }
}
/* circle chart */
</style>