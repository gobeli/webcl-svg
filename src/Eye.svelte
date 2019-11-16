<style>
	svg {
    height: 80px;
    background-color: #eee;
    border-radius: 50%;
	}
</style>

<script>
  let rectEl;
  let eyeOffset = {
    x: 0,
    y: 0
  }

  setInterval(() => {
    // only blink if eyes are not already closed
    if (closed) {
      return
    }
    closed = true;
    setTimeout(() => closed = false, 200);
  }, 3200)

  const onMouseMove = evt => {
    const rect = rectEl.getBoundingClientRect();
    const xo = rect.x + rect.width/2;  // x-origin
    const yo = rect.y + rect.height/2; // y-origin
    const xm = evt.clientX - xo; // the normalized x/y coords to work with
    const ym = evt.clientY - yo;
    const xmax = rect.width/1.5;
    const ymax = rect.height/2;
    const widestFocus = 400; // when x is so far away, the eye is maximal extended
    const scaledX = xm * (xmax / widestFocus );
    let xe = xm > 0
                ? Math.min( xmax, scaledX)
                : Math.max(-xmax, scaledX);
    const scaledY = ym * (ymax / widestFocus );
    let ye = ym > 0
                ? Math.min( ymax, scaledY)
                : Math.max(-ymax, scaledY);
    if (xe*xe + ye*ye > xmax * ymax) {
        xe *= 0.9;
        ye *= 0.9;
    }
    eyeOffset = { x: xe, y: ye };
  }

  export let closed;
</script>
<svelte:window on:mousemove={onMouseMove}/>

<svg viewBox="0 0 120 120" on:click={() => closed = !closed}>
  <filter id="shadow">
    <feGaussianBlur in="SourceAlpha" stdDeviation="3" />
    <feOffset       dx="0" dy="8" />
    <feColorMatrix  type="matrix"
                    values="0 0 0 0  0
                            0 0 0 0  0
                            0 0 0 0  0
                            0 0 0 .5 0"/>
    <feBlend        in="SourceGraphic" mode="normal"/>
  </filter>
  <radialGradient id="gradient1" gradientUnits="objectBoundingBox" cx="50%" cy="50%" r="50%">
    <stop offset= "38%" stop-color="#000000" stop-opacity="1" />
    <stop offset= "46%" stop-color="#073F80" stop-opacity="1" />
    <stop offset= "90%" stop-color="#8EC0DD" stop-opacity="1" />
    <stop offset="100%" stop-color="#2F3A46" stop-opacity="1" />
  </radialGradient>
  <g class="iris" style="transform: translateX({eyeOffset.x}px) translateY({eyeOffset.y}px)">
    <ellipse bind:this={rectEl} cx="60" cy="60" rx="30" ry="30" opacity="1" fill="url(#gradient1)" />
    <ellipse cx="50" cy="50" rx="7" ry="7" opacity="1" fill="#FFFFFF" fill-opacity="0.8"/>
  </g>
  <g class="lids">
    <path d="M0 60 A60,60 0 0,1 120,60 A60,{closed ? 40 : 30} 0 0,{closed ? 1 : 0} 0,60 Z" fill="#FDDC99" fill-opacity="1" filter="url(#shadow)" />
    <path d="M0 60 A60,60 0 0,0 120,60 A60,40 0 0,1 0,60 Z" fill="#F4CB76" fill-opacity="1" />
  </g>
</svg>