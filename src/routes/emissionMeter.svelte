<!-- add script to collect emissions -->

<script>
    import HappyCar from "../assets/emission_meter/Happy Car.svg";
    import NeutralCar from "../assets/emission_meter/Neutral Car.svg";
    import SadCar from "../assets/emission_meter/Sad Car.svg";

    var emissions = 19;
</script>



<div class="container">
    <h1>Total Yearly Emmissions</h1>
    <div class="progress-bar"/>
    <h1>Current Car Emissions</h1>
</div>

<!-- have an if svelte condition for which of the three to appear -->
<div class="car_icon">
    {#if emissions < 10}
        <img src="{HappyCar}">
    {:else if emissions < 20}
        <img src="{NeutralCar}"/>
    {:else}
        <img src="{SadCar}"/>
    {/if}
</div>


<style>
    .container {
        padding: 50% 10% 5%;
        align-items: center;
    }

    h1 {
        text-align: center;
        font-family: 'Courier New', Courier, monospace;
        font-size: 5.4vw;
    }

    .progress-bar {
        position: relative;
        width: 75vw;
        height: 75vw;
        border-radius: 50%;
        transform: rotate(-90deg);
        background: 
            linear-gradient(to left, transparent 50%, white 50%),
            radial-gradient(closest-side, white 90%, transparent 80% 100%),
            conic-gradient(#3FF651 30%, #D9D9D9 0); /* first percentage controls value */
    }

    .car_icon {
        position: absolute;

        align-items: center;
        z-index: 1;
        top: 120vw;
        left: 35vw;
    }

    .car_icon img {
        width: 30vw;
        height: auto;
    }
</style>