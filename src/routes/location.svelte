<script>
    let timePassed = 0;
    let prev;
    let totalD = 0;
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

        if (prev !== undefined) {
        totalD += distance(
            prev.coords.latitude,
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
        clearTimeout(tid);
    }

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

    export let dist = totalD;
</script>

<button on:click={startLocation}>Start Tracking</button>
<button on:click={stopLocation}>Stop Tracking</button>

<div class="dropdown">
    <button class="btn {isClicked ? 'active' : ''}" on:click={toggle}>
        <span id="text">{name}</span>
        <span class="arrow"></span>
    </button>
    <ul class="content">
        <li id="time">Time Elapsed: {timePassed} seconds</li>
        <li id="dist">Distance: {totalD} km</li>
        <li id="mpg">Average MPG: </li>
        <li id="speed">Average Speed: {totalD/timePassed/10} km/s</li>
        <li id="emission">Carbon Emission (g): </li>
    </ul>
</div>

<style>
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
        padding-right: 2.5em;
        margin-top: 0;
        border-radius: 0.5em;

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
    }

    .active + .content {
        max-height:30em;
        border: 1px solid black;
    }

    .active > .arrow {
        transform: rotate(180deg);
    }
</style>