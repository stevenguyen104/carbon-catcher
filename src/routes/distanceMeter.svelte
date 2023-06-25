<script>
// @ts-nocheck

    export let dist;
</script>

<div class="container">
    <h1 class="distance-text">Distance Travelled</h1>
    <div class="bar">
        <div class="progress" style="width:{dist/10000 * 100}%"></div> 
        <!-- progress width change according to distance from api -->
    </div>
</div>

<style>
    .container {
        position: relative;
        align-items: center;
        padding: 1vw;
        margin-bottom: 10vw;
    }

    .distance-text {
        font-family: 'Courier New', Courier, monospace;
        font-size: 5.4vw;
        text-align: center;
    }

    .bar {
        position: absolute;
        right: 10%;
        left: 10%;

        width: 80%;
        height: 3.5vw;
        background-color: #D9D9D9;
        border-radius: 2vw;
    }

    .progress {
        max-width: 100%;
        background-color:cadetblue;
        height: 3.5vw;
        border-radius: 2vw;
    }
</style>