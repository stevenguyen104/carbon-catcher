<script>
    import DistanceMeter from './distanceMeter.svelte';
    import EmissionMeter from './emissionMeter.svelte';

    let timePassed = 0;
    // @ts-ignore
    let prev;
    let totalD = 0;
    // @ts-ignore
    let tid;

    function startLocation() {
        tid = setTimeout(getLocation, 10000);
    }

    function getLocation() {
        if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition);
        } else {
        console.log("Geolocation is not supported by this browser.");
        }
        timePassed += 10;
        tid = setTimeout(getLocation, 10000);
    }

    // @ts-ignore
    function showPosition(position) {
        console.log(
        "Latitude: " +
            position.coords.latitude +
            "<br>Longitude: " +
            position.coords.longitude +
            "<br>Time: " +
            timePassed +
            " sec"
        );

        // @ts-ignore
        if (prev !== undefined) {
        totalD += distance(
            // @ts-ignore
            prev.coords.latitude,
            // @ts-ignore
            prev.coords.longitude,
            position.coords.latitude,
            position.coords.longitude
        );
        } else {
        prev = position;
        }
    }

    function stopLocation() {
        console.log("Total distance traveled is: " + totalD + " km");
        // @ts-ignore
        clearTimeout(tid);
    }

    // @ts-ignore
    function distance(lat1, lon1, lat2, lon2) {
        const p = 0.017453292519943295; // Math.PI / 180
        const c = Math.cos;
        const a =
        0.5 -
        c((lat2 - lat1) * p) / 2 +
        c(lat1 * p) * c(lat2 * p) * (1 - c((lon2 - lon1) * p)) / 2;

        return 12742 * Math.asin(Math.sqrt(a)); // 2 * R; R = 6371 km
    }

    import { onMount } from "svelte";

    let d = new Date();
    let name =
        "Today's Session " +
        (d.getMonth() + 1) +
        "-" +
        d.getDate() +
        "-" +
        d.getFullYear() +
        ":";

    onMount(() => {
        console.log(name);
        console.log("Time: " + timePassed + " seconds");
        console.log("Distance: " + totalD + " km");
        console.log("Speed: " + 0 + " km/s");
    });

    let isClicked = false;

    function toggle() {
        isClicked = !isClicked;
    }

    let start = false;

    let co2emissions = 0;

    function toggleStart() {
        start = !start;
        if (start) {
            startLocation();
        } else {
            stopLocation();
        }
    }
    
    function resetAll() {
        timePassed = 0;
        // @ts-ignore
        prev = undefined;
        totalD = 0;
        // @ts-ignore
        tid = undefined;
    }
</script>

<h2 class="prompt"> Enter your car's CO2 Emissions in grams per kilometers </h2>
<input id="number" type="number" bind:value={co2emissions}/>

<button class="startButton {start ? 'start' : ''}" on:click={toggleStart}>{!start ? "Start Tracking" : "Stop Tracking"}</button>
<button class="reset" on:click={resetAll}>Reset</button>

<EmissionMeter emissions={co2emissions * totalD}/>

<div>
    <h1 class="current">{(co2emissions == null || totalD == 0) ? 0.0 : co2emissions * totalD / 1000} kg</h1>
</div>

<DistanceMeter dist={totalD}/>
<div class="dropdown">
    <button class="btn {isClicked ? 'active' : ''}" on:click={toggle}>
        <span id="text">{name}</span>
        <span class="arrow"></span>
    </button>
    <ul class="content">
        <li id="time">Time Elapsed: {timePassed} seconds</li>
        <li id="dist">Distance: {totalD} km</li>
        <li id="speed">Average Speed: {(timePassed == 0) ? 0 : totalD/timePassed/10} km/s</li>
        <li id="emission">Carbon Emission (g): {co2emissions * totalD} g</li>
    </ul>
</div>

<style>
    .prompt {
        margin-top: 20vw;
        font-family: 'Courier New', Courier, monospace;
        font-size: 5vw;
        text-align: center;
    }

    input {
        width: 75%;
        height: 7vw;
        margin-left: 12.5vw;
        margin-right: 12.5vw;
        margin-bottom: 10vw;

        border-radius: 1vw;
        border-color: #D9D9D9;

        font-size: 5vw;
        font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    }

    .startButton {
        width: 33vw;
        height: 15vw;

        position: absolute;
        left: 15vw;

        font-size: 5vw;
        font-family: 'Courier New', Courier, monospace;
        font-weight: bold;

        background-color: #B8FBA2;
        border-radius: 2vw;
    }

    .start {
        background-color: #F8D8F0;
    }

    .reset {
        width: 33vw;
        height: 15vw;

        position: absolute;
        right: 15vw;

        font-size: 5vw;
        font-family: 'Courier New', Courier, monospace;
        font-weight: bold;

        background-color: #F8D8E0;
        border-radius: 2vw;
    }

    .current {
        text-align: center;
        background-color: #D9D9D9;
        margin-left: 15vw;
        margin-right: 15vw;
        border-radius: 4vw;
        margin-top: 0;
    }

    .dropdown {
        max-width: 85vw;
        margin: 6vw;
        position: relative;
        width: 100%;
    }
    .btn {
        background: #BECFFE;
        font-size: 18px;
        font-family:'Courier New', Courier, monospace;
        font-weight: bold;
        width: 100%;
        border: none;
        color: #fff;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0.7em 0.5em;
        border-radius: 0.5em;
        cursor: pointer;
    }
    #text {
        font-size: 4vw;
        color: black;
    }
    .arrow {
        border-left: 2vw solid transparent;
        border-right: 2vw solid transparent;
        border-top: 2.3vw solid black;
        transition: transform ease-in-out 0.3s;
    }
    .content {
        text-align: left;
        padding: 0;
        margin-right: 6vw;
        margin-top: 3vw;
        border-radius: 0.5em;

        max-width: 85vw;
        width: 100%;

        background-color:#BECFFE;
        max-height:0;
        overflow:hidden;
        position:absolute;
        -webkit-transition:max-height .2s ease-in-out .2s;
        -moz-transition:max-height .2s ease-in-out .2s;
        -o-transition:max-height .2s ease-in-out .2s;
        -ms-transition:max-height .2s ease-in-out .2s;
        transition:max-height .2s ease-in-out .2s;
    }
    .content li {
        padding:2.5vw;
        width: 67vw;
        list-style: none;
        font-size: 4vw;
        font-family:'Courier New', Courier, monospace;
        font-weight: 600;
    }

    .active + .content {
        max-height:30em;
        border: 1px solid black;
    }

    .active > .arrow {
        transform: rotate(180deg);
    }
</style>