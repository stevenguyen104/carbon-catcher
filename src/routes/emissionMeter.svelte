<!-- add script to collect emissions -->

<script>
    import HappyCar from "../assets/emission_meter/Happy Car.svg";
    import NeutralCar from "../assets/emission_meter/Neutral Car.svg";
    import SadCar from "../assets/emission_meter/Sad Car.svg";

</script>



<div class="container">
    <h1>Total Yearly Emmissions</h1>
    <div class="progress-bar"/>
</div>

<!-- have an if svelte condition for which of the three to appear -->
<div class="car_icon">
    <img src="{HappyCar}"/>
    <img src="{NeutralCar}"/>
    <img src="{SadCar}"/>

</div>


<style>
    .container {
        padding: 50% 10% 5%;
        align-items: center;
    }

    h1 {
        text-align: center;
        font-family: 'Courier New', Courier, monospace;
        font-size: 5.5vw;
    }

    .progress-bar {
        position: relative;
        width: 75vw;
        height: 75vw;
        border-radius: 50%;
        background: 
            radial-gradient(closest-side, white 79%, transparent 80% 100%),
            conic-gradient(#3FF651 20%, #D9D9D9 0);    
    }
</style>