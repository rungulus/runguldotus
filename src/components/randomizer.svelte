<script>
  import { writable } from "svelte/store";
  import { onMount } from "svelte";
  import { SymbolDisplayPartKind } from "typescript";

  let player;
  let buttonText = writable("");
  let playerLoaded = false;
  let randURLs = [];
  let videoTitle = writable("");

  onMount(async () => {
    try {
      // Fetch the video IDs from your CDN
      const response = await fetch(
        "https://cdn.rungus.zone/randomizerids.json",
      );
      // Update randURLs with the fetched data
      randURLs = await response.json();
    } catch (error) {
      console.error("Failed to fetch video IDs:", error);
    }
  });

  function loadScript() {
    if (!window.YT) {
      const tag = document.createElement("script");
      tag.src = "https://www.youtube.com/iframe_api";
      const firstScriptTag = document.getElementsByTagName("script")[0];
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

      window.onYouTubeIframeAPIReady = onYouTubeIframeAPIReady;
    }
  }

  loadScript();

  function onYouTubeIframeAPIReady() {
    player = new YT.Player("player", {
      height: "720",
      width: "1280",
      videoId: "", // Initialize the player without a video ID
      playerVars: {
        'autoplay': 0,
        'controls': 0, 
        'rel' : 0,
        'fs' : 0,
        'playsinline': 1,
      },
      events: {
        onReady: onPlayerReady,
        onStateChange: onPlayerStateChange,
        onError: onPlayerError,
      },
    });
  }
  // Load the YouTube IFrame API script
  if (!window.YT) {
    const tag = document.createElement("script");
    tag.src = "https://www.youtube.com/iframe_api";
    const firstScriptTag = document.getElementsByTagName("script")[0];
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
  }

  window.onYouTubeIframeAPIReady = onYouTubeIframeAPIReady;

  function onPlayerReady(event) {
    // Player is ready but not loaded with a video yet
    playerLoaded = true;
  }

  function onPlayerStateChange(event) {
    if (playerLoaded) {
      if (event.data === YT.PlayerState.PLAYING) {
        videoTitle.set(player.getVideoData().title);
      }
    }
    if (playerLoaded && event.data === YT.PlayerState.ENDED) {
      reroll();
    }
  }

  function onPlayerError(event) {
    if ([2, 5, 9, 11, 101, 150].includes(event.data)) {
      console.error(`YouTube Player error: ${event.data}`);
      setTimeout(reroll, 500);
    }
  }

  function getRandomVideoId() {
    // Randomly select a video ID from the `randURLs` array
    return randURLs[Math.floor(Math.random() * randURLs.length)];
  }

  function loadYT() {
    if (playerLoaded && randURLs.length > 0) {
      reroll(); // Use reroll for the initial load as well
    }
  }

  function reroll() {
    const randomvideo = getRandomVideoId();
    player.loadVideoById(randomvideo);
    buttonText.set("reroll");
  }
</script>

<div class="">
  <p class="pb-2">
    <span
      class="gradient-text text-center font-extrabold tracking-tighter py-10"
      >{$videoTitle}</span
    >
    <button
      class="select-none 
      rounded-lg bg-gradient-to-tr from-indigo-500 from-10% via-sky-500 via-30% to-green-500 
      py-2 px-2 
      text-center align-middle font-sans text-xs 
      text-white 
      shadow-md shadow-gray-900/10 
      transition-all hover:shadow-lg 
      hover:shadow-gray-900/20 
      active:opacity-[0.85] 
      disabled:pointer-events-none 
      disabled:opacity-50 
      disabled:shadow-none 
      float-end"
      on:click={loadYT}
      type="button"
    >
      {$buttonText}
    </button>
  </p>
  <div class="">
    <div id="player"></div>
  </div>
</div>
