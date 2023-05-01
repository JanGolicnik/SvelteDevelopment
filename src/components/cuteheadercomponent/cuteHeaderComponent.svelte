<script lang="ts">

    export let text = 'Hello World';
    export let fontFamily = "Arial";
    export let fontWeight = 100;
    export let padding = 1;
    export let centered = false;
    export let spacing = .8;

    let lines = text.split(' ').map(d=>d.split(''));
    const columns = Math.max(...lines.map(d=>d.length)) * spacing;
    const rows = lines.length; 

    const width = columns + padding * 2;
    const height = rows + padding * 2;
    const overlay = (width ** 2 + height ** 2) ** 0.5;

    const duration = 1;
    const delays = 0.1;
    
    const characters = lines.reduce((acc, curr, y) => {
        return [
            ...acc,
            ...curr.reduce((a,c,x)=>{
                let xpos = x * spacing;
                if(centered){
                    xpos +=  (width - padding*2)/2 - (curr.length/2) * spacing;
                }
                return [
                    ...a,
                    {
                        text: c,
                        x: xpos,
                        y,
                        delay: delays * (x + y)
                    }
                ];
            }, [])
        ];
    }, []);

    const id = text.toLowerCase().replace(/\s/g, '-');

</script>

<h1>
    <svg viewBox="0 0 {width} {height}">
        <title id="title-{id}">{text}</title>
        <defs>
            <radialGradient id="gradient-{id}" spreadMethod="reflect" x1=0 x2=0.5>
                <stop stop-color="rgb(178,164,255)" offset="0" />
				<stop stop-color="rgb(255,180,180)" offset="17%" />
				<stop stop-color="rgb(255,210,152)" offset="33%" />
				<stop stop-color="rgb(255,245,157)" offset="50%" />
				<stop stop-color="rgb(255,222,180)" offset="66%" />
				<stop stop-color="rgb(255,180,180)" offset="83%" />
				<stop stop-color="rgb(178,164,255)" offset="100%" />
            </radialGradient>
            
            <rect
                id=overlay-{id}
                width={overlay}
                height={overlay}
                fill=url(#gradient-{id})
                stroke=url(#gradient-{id})
            />

            <mask id=mask-{id}>
                <rect {width} {height} fill=black/>
                <g transform="translate({padding + 0.5} {padding + 0.5})">
                    <g
                        fill=white
                        text-anchor=middle
                        dominant-baseline=middle
                        font-size=0.5
                        font-family={fontFamily}
                        font-weight={fontWeight}
                    >
                     {#each characters as char}
                        <g transform="translate({char.x} {char.y})">
                            <g
                            class=transform
                            style:animation-duration={duration}s
                            style:animation-delay="{char.delay + 0.1}s"
                            >
                                <text y=0.1>{char.text}</text>
                            </g>
                        </g>
                     {/each}
                    </g>
                </g>
            </mask>
        </defs>

        <g mask="url(#mask-{id})">
            <g transform="translate({width / 2} {height / 2})">
                <g transform="translate({-overlay/2}, {-overlay/2})">
                    <use x={-overlay} href="overlay-{id}"/>
                    <use href="#overlay-{id}"/>
                </g>
            </g>
        </g>
    </svg>
</h1>


<style>
    svg {
        display: block;
    }

    @media (prefers-reduced-motion: no-preference){
        .transform{
            animation: transform cubic-bezier(.64,1.6,.51,1.01) both;
        }

        @keyframes transform {
            from {
                transform:scale(0) rotate(0turn);
            }
            to{
                transform: scale(1) rotate(1turn);
            }
        }
    }

</style>