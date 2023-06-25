<!-- add script to collect emissions -->

<script>
// @ts-nocheck

    import HappyCar from "../assets/emission_meter/Happy Car.svg";
    import NeutralCar from "../assets/emission_meter/Neutral Car.svg";
    import SadCar from "../assets/emission_meter/Sad Car.svg";

    export let emissions;
</script>



<div class="container">
    <h1>Daily Recommended Emmissions</h1>
    <div class="progress-bar" style="background: 
    linear-gradient(to left, transparent 50%, white 50%),
    radial-gradient(closest-side, white 90%, transparent 80% 100%),
    conic-gradient(#3FF651 {emissions/12600 * 100 / 2}%, #D9D9D9 0); /* first percentage controls value */"/>
    <h1 class="current">Current Car Emissions</h1>
</div>

<!-- have an if svelte condition for which of the three to appear -->
<div class="car_icon">
    {#if emissions/12600 < .33}
        <img src="{HappyCar}" alt="img">
    {:else if emissions/12600 < .66}
        <img src="{NeutralCar}" alt="img"/>
    {:else}
        <img src="{SadCar}" alt="img"/>
    {/if}
</div>


<style>
    .container {
        padding: 20% 10% 5%;
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

        z-index: -1;
    }

    .car_icon {
        position: absolute;

        align-items: center;
        z-index: 1;
        top: 105vw;
        left: 35vw;

        border-width: 0;
    }

    .car_icon img {
        width: 30vw;
        height: auto;
    }

    .current {
        margin-top: -20vw;
        margin-bottom: 0;
    } 
</style>