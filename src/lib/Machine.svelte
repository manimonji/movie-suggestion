<script>
    import { onMount } from "svelte";

    let start = new Date();
    let timePassed;
    onMount(() => setInterval(() => timePassed = new Date() - start, 10)); 

    export let classNames = "";
</script>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 150 200" class={classNames}>
    <g transform="translate(0, 5)">
        <path d="M 0 0 L 20 40 L 100 40 L 120 0 Z" fill="hsl(0, 0%, 50%)" stroke="#000" stroke-width="4" stroke-linejoin="round" stroke-linecap="round"/>
        <rect x="100" y="80" width="40" height="100" fill="hsl(0, 0%, 50%)" stroke="#000" stroke-width="4" stroke-linejoin="round" stroke-linecap="round"/>
        <g transform="translate(0,30)">
            <rect x="10" y="10" width="100" height="150" rx="10" ry="10" style="fill: hsl(40deg, 100%, 60%); stroke: hsl(40deg, 10%, 10%); stroke-width: 5;"/>
            <g transform="translate(80, 40)">
                <circle cx="0" cy="0" r="15" style="fill: #fff; stroke: #000; stroke-width: 5;"/>
                <g transform="rotate(-50, 0, 5)">
                    <circle r="4" cy="5" style="fill: hsl(0deg, 70%, 50%);"/>
                    <path d="M -2 5 L 2 5 L 1 -7 Q 0 -9 -1 -7 Z" style="fill: hsl(0deg, 70%, 50%);"/>
                </g>
                <circle cx="0" cy="0" r="15" style="fill: transparent; stroke: #000; stroke-width: 5;"/>
            </g>
            <g transform={`translate(80, 90)`}>
                <g transform={`rotate(${((timePassed / 1000) % 1) * 360})`}>            
                    <circle cx="0" cy="0" r="15" style="fill: #fff; stroke: #000; stroke-width: 5;"/>
                    <circle r="2"/>
                    <circle r="3" cy="8" transform="rotate(0, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(60, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(120, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(180, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(240, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(300, 0, 0)"/> 
                </g>
                <line x1="-15.5" x2="-11.25" y1="0" y2="40" stroke="black" stroke-width="4"/>
                <line x1="15.5" x2="11.25" y1="0" y2="40" stroke="black" stroke-width="4"/>
                <g transform={`translate(0, 40), scale(0.75), rotate(${((timePassed / 500) % 1) * 360})`}>
                    <circle cx="0" cy="0" r="15" style="fill: #fff; stroke: #000; stroke-width: 5;"/>
                    <circle r="2"/>
                    <circle r="3" cy="8" transform="rotate(0, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(60, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(120, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(180, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(240, 0, 0)"/>
                    <circle r="3" cy="8" transform="rotate(300, 0, 0)"/>
                </g>
            </g>
            <g transform="translate(22.5, 22.5)">
                <rect width="30" height="125" rx="5" ry="5" />
                <g transform="translate(5, 5)">
                    <rect width="20" height="10" fill="hsl(0deg, 20%, 30%)"/>
                    <rect width="20" height="10" fill="hsl(20deg, 20%, 30%)" y="15"/>
                    <rect width="20" height="10" fill="hsl(40deg, 20%, 30%)" y="30"/>
                    <rect width="20" height="10" fill="hsl(60deg, 20%, 30%)" y="45"/>
                    <rect width="20" height="10" fill="hsl(80deg, 20%, 30%)" y="60"/>
                    <rect width="20" height="10" fill="hsl(100deg, 20%, 30%)" y="75"/>
                    <rect width="20" height="10" fill="hsl(120deg, 20%, 30%)" y="90"/>
                    <rect width="20" height="10" fill="hsl(140deg, 20%, 30%)" y="105"/>
                </g>
            </g>
        </g>
    </g>
</svg>