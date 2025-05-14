<script>
  let allApps = [
    { src: "/icons/app.png", name: "Sketch" },
    { src: "/icons/AppIcon 2-i.png", name: "Wave" },
    { src: "/icons/AppIcon 3-i.png", name: "Ghostty" },
    { src: "/icons/AppIcon 3.png", name: "VSCode" },
    { src: "/icons/AppIcon 4-i.png", name: "Inna" },
    { src: "/icons/AppIcon 4.png", name: "Bartender" },
    { src: "/icons/AppIcon 5-i.png", name: "TV" },
    { src: "/icons/AppIcon 5.png", name: "Tables Plus" },
    { src: "/icons/AppIcon 6-i.png", name: "OBS" },
    { src: "/icons/AppIcon 6.png", name: "Wes Bos" },
    { src: "/icons/AppIcon 7.png", name: "Popsicle" },
    { src: "/icons/AppIcon 8.png", name: "Mail" },
    { src: "/icons/AppIcon-i.png", name: "Sewing" },
    { src: "/icons/AppIcon-Release-i.png", name: "Rayjay" },
    { src: "/icons/appIcon.png", name: "Big Staxxx" },
    { src: "/icons/Code.png", name: "VSCode" },
    { src: "/icons/Cursor.png", name: "Viber" },
    { src: "/icons/cyberduck-application-rect.png", name: "Scary Duck" },
    { src: "/icons/electron-i.png", name: "Ligma" },
    { src: "/icons/electron.png", name: "F Word" },
    { src: "/icons/firefox.png", name: "Fox Fire" },
    { src: "/icons/Icon 2-i.png", name: "Bambu" },
    { src: "/icons/icon-i.png", name: "Hyper" },
    { src: "/icons/icon.png", name: "Eyeballer" },
    { src: "/icons/Trae-i.png", name: "Orange Box" },
    { src: "/icons/Warp-i.png", name: "Warp" },
    { src: "/icons/Windsurf.png", name: "Windsurfer" },
    { src: "/icons/Xcode.png", name: "XCode" },
    { src: "/icons/Zed.png", name: "Zeesh" },
  ];

  let activeApps = $state([...allApps]);
  let discardedApps = $state([]);

  function removeApp(index) {
    const [removed] = activeApps.splice(index, 1);
    discardedApps = [...discardedApps, removed];
  }

  function addApp() {
    if (discardedApps.length === 0) return;
    const [added] = discardedApps.splice(0, 1);
    activeApps = [...activeApps, added];
  }
</script>

<div class="parent">
  <div class="container-2">
    <div class="container" style="--total: {activeApps.length}">
      {#each activeApps as app, i}
        <div class="square" style="--i: {i}">
          <img src={app.src} />
        </div>
        <p>{app.name}</p>
      {/each}
    </div>
    <div class="doughnut"></div>
  </div>
  <div class="controls">
    <button onclick={() => removeApp(0)} disabled={activeApps.length === 0}>-</button>
    <button onclick={addApp} disabled={discardedApps.length === 0}>+</button>
  </div>
</div>

<style>
  :global(body) {
    background: #333;
    position: relative;
  }

  .parent {
    scale: 2;
    transform-origin: top;
    padding: 40px;
    position: relative;
    background-image: url(/grid.svg), url(/mesh.svg);
    background-repeat: repeat, no-repeat;
    background-size: 50%, cover;
    width: 300px;
    margin: 0 auto;
    border-radius: 5px;
    height: 300px;
  }

  .controls {
    display: flex;
    gap: 1rem;
    justify-content: center;
    margin-top: 1rem;
  }

  .controls button {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: none;
    background: #4e3f52;
    color: white;
    font-size: 1.5rem;
    cursor: pointer;
    transition: all 0.2s ease;
  }

  .controls button:disabled {
    background: #666;
    cursor: not-allowed;
  }

  .controls button:hover:not(:disabled) {
    background: #358af6;
  }

  .square {
    --offset: calc(var(--i) / var(--total) * 100%);
    --delta: 0%;
    width: 50px;
    height: 50px;
    border-radius: 10px;
    offset-rotate: 0deg;
    offset-path: circle(100px at 150px 150px);
    offset-distance: calc(var(--offset) + var(--delta));
    transition: offset-distance 0.2s ease;
    position: relative;

    img {
      width: 100%;
      scale: 1.3;
    }

    &::before {
      opacity: 0;
      content: "";
      border-radius: 10px;
      height: 90px;
      width: 38px;
      position: absolute;
      offset-path: circle(1px at 25px 25px);
      offset-distance: calc(var(--offset) + var(--delta));
      background: #358af6;
      clip-path: polygon(0 0, 100% 0, 80% 100%, 20% 100%);
    }
    &:hover {
      z-index: 6;
    }
    &:hover::before {
      opacity: 1;
    }
  }

  .square:hover + p + .square,
  .square:first-child:has(~ .square:hover + p:last-child) {
    z-index: 5;
    --delta: 5%;
  }
  /* 14 hover 1 2%, */
  .square:hover + p + .square + p + .square,
  .square:first-child + p + .square:has(~ .square:hover + p:last-child),
  .square:first-child:has(~ .square:hover + p + .square + p:last-child) {
    z-index: 2;
    --delta: 3%;
  }

  .square:hover + p + .square + p + .square + p + .square,
  .square:first-child + p + .square + p + .square:has(~ .square:hover + p:last-child),
  .square:first-child:has(~ .square:hover + p + .square + p + .square + p:last-child),
  .square:first-child + p + .square:has(~ .square:hover + p + .square + p:last-child) {
    z-index: 1;
    --delta: 1%;
  }
  .square:has(+ p + .square:hover),
  .square:first-child:hover ~ .square:has(+ p:last-child) {
    z-index: 5;
    --delta: -5%;
  }
  .square:has(+ p + .square + p + .square:hover),
  .square:first-child:hover ~ .square:has(+ p + .square + p:last-child),
  .square:first-child + p + .square:hover ~ .square:has(+ p:last-child) {
    z-index: 2;
    --delta: -3%;
  }

  .square:has(+ p + .square + p + .square + p + .square:hover),
  .square:first-child:hover ~ .square:has(+ p + .square + p + .square + p:last-child),
  .square:first-child + p + .square:hover ~ .square:has(+ p + .square + p:last-child),
  .square:first-child + p + .square + p + .square:hover ~ .square:has(+ p:last-child) {
    z-index: 1;
    --delta: -1%;
  }

  .container {
    inset: 0;
    position: relative;
    z-index: 2;
    height: 300px;
    p {
      position: absolute;
      z-index: 100;
      top: 50%;
      left: 50%;
      translate: -50% -50%;
      background: #4e3f52;
      border-radius: 7px;
      color: white;
      font-weight: 600;
      text-shadow: 0 0 1px rgb(0 0 0 / 0.2);
      font-family: "SF Pro Rounded";
      box-shadow:
        inset 0 1px 1.5px #4e3f52,
        inset 0 1px 0.5px rgb(255 255 255 / 0.5);
      font-size: 10px;
      padding: 2px 4px;
      margin: 0;
      opacity: 0;
    }
  }
  .square:hover + p {
    opacity: 1;
  }

  .container-2 {
    position: relative;
  }

  .doughnut {
    inset: 0;
    z-index: 1;
    position: absolute;
    background: radial-gradient(circle, transparent 25%, #eae9e7 20%);
    border-radius: 100%;
    height: 300px;
    width: 300px;
    filter: drop-shadow(0 0 1px #bab6ba);
  }
</style>
