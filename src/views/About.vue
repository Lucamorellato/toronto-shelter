<template>
  <div class="about wrapper">
    <h1>About</h1>
    <p>All of the data used here is provided by the City of Toronto <a href="https://open.toronto.ca/">Open Data</a> project. You can find the specific dataset here: <a href="https://open.toronto.ca/dataset/daily-shelter-occupancy/">Daily Shelter Occupancy.</a> Shelter Occupancy TO was not created to be a tool for finding an adequate shelter. The intention of this site was to highlight the occupancy rates of Toroto's shelters.</p>
    <p>The numbers include all shelters whose occupancy was reported most recently. The number of shelters open and beds reported will change as the year progresses, and the data is not always updated every day.</p>
  </div>
</template>
<style scoped>
  p {
    width: 70%;
  }
  h1 {
    padding-bottom: 10rem;
  }
  @media (max-width: 680px) {
    p {
      width: 100%;
    }
  }
</style>
