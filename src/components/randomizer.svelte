<script>
  import { writable } from "svelte/store";
  import { onMount } from "svelte";

  let player;
  let buttonText = writable("");
  let playerLoaded = false;
  let randURLs = [];
  let videoTitle = writable("");
  let useTrueRandom = false;
  let isLoading = writable(false);
  let invalidIDsCount = 0;

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

  let showAlertModal = false; // A flag to control the display of the modal alert

  function toggleTrueRandom() {
    useTrueRandom = !useTrueRandom;
    if (useTrueRandom && !showAlertModal) {
      showAlertModal = true; // Show the modal once
    }
  }

  // Function to hide the modal
  function hideModal() {
    showAlertModal = false;
  }

  function onYouTubeIframeAPIReady() {
    player = new YT.Player("player", {
      height: "720",
      width: "1280",
      videoId: "", // Initialize the player without a video ID
      playerVars: {
        autoplay: 0,
        controls: 1,
        rel: 0,
        fs: 1,
        playsinline: 1,
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
        document.getElementById("player").style.display = "";
        invalidIDsCount = 0; // Reset the invalid video IDs counter
        isLoading.set(false);
      }
    }
    if (playerLoaded && event.data === YT.PlayerState.ENDED) {
      reroll();
    }
    if (
      event.data === YT.PlayerState.PLAYING ||
      event.data === YT.PlayerState.ENDED ||
      event.data === YT.PlayerState.CUED
    ) {
      isLoading.set(false); // Set to false when the video starts playing, ends, or is cued
    }
  }

  function onPlayerError(event) {
    invalidIDsCount++;
    if (invalidIDsCount >= 2 && useTrueRandom) {
      isLoading.set(true); // Show loading indicator
      document.getElementById("player").style.display = "none"; // Hide player
      document.getElementById("videoidfailcount").innerHTML = invalidIDsCount;
      continueRerolling();
    }
    continueRerolling();
  }

  function continueRerolling() {
    // Consider rechecking if the player is still in an error state before rerolling
    setTimeout(
      () => {
        reroll();
      },
      useTrueRandom ? 0 : 1,
    );
  }

  function reroll() {
    // Always attempt to reroll even if isLoading is true
    const randomvideo = getRandomVideoId();
    player.loadVideoById(randomvideo);
    buttonText.set("reroll");
  }

  function getRandomVideoId() {
    if (useTrueRandom) {
      return trueRandomVideoId();
    } else {
      return randURLs[Math.floor(Math.random() * randURLs.length)];
    }
  }

  function loadYT() {
    if (playerLoaded && randURLs.length > 0) {
      reroll(); // Use reroll for the initial load as well
    }
  }

  function trueRandomVideoId() {
    var elements =
      "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-_";
    var videoIdLength = 11;
    var videoId = "";

    for (var i = 0; i < videoIdLength; i++) {
      var randomIndex = Math.floor(Math.random() * elements.length);
      videoId += elements[randomIndex];
    }

    var fullURL = "https://www.youtube-nocookie.com/embed/" + videoId;
    console.log("video id:", videoId);
    console.log("video url:", fullURL);
    return videoId;
  }
</script>

<div class="">
  <p class="">
    <span
      class="gradient-text text-center font-extrabold tracking-tighter text-xl py-10 mb-3"
    >
      the randomizer</span
    ><span class="gradient-text text-xs ps-1">v4</span>
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

  <div class="mt-3">
    <div
      id="popup-modal"
      tabindex="-1"
      class=" overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full"
      class:hidden={!showAlertModal}
    >
      <div class="relative p-4 w-full max-w-md max-h-full">
        <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
          <button
            type="button"
            class="absolute top-3 end-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
            on:click={hideModal}
          >
            <svg
              class="w-3 h-3"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 14 14"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"
              />
            </svg>
            <span class="sr-only">Close modal</span>
          </button>
          <div class="p-4 md:p-5 text-center">
            <svg
              class="mx-auto mb-1 text-gray-400 w-12 h-12 dark:text-gray-200"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 20 20"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M10 11V6m0 8h.01M19 10a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
              />
            </svg>
            <h3
              class="mb-1 text-xl font-normal text-gray-100 dark:text-gray-100"
            >
              true random mode is <span class="gradient-text font-extrabold"
                >experimental</span
              >!
            </h3>
            <p class="text-gray-300">
              it will take a long time to find a video.
            </p>
            <p class="text-gray-300">
              <span class="font-bold">you may get banned</span> by youtube for the
              amount of requests!
            </p>
            <h4
              class="mb-3 text-lg font-normal text-gray-100 dark:text-gray-100 underline"
            >
              use caution!
            </h4>
            <button
              on:click={hideModal}
              type="button"
              class="text-white bg-red-600 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 dark:focus:ring-red-800 font-medium rounded-lg text-sm inline-flex items-center px-5 py-2.5 text-center"
            >
              ok, got it!
            </button>
          </div>
        </div>
      </div>
    </div>
    {#if $isLoading}
      <section
        class="bg-white dark:bg-gray-900 z-100 pos-absolute"
        style="height:720px;width:1280px;"
      >
        <div
          class="py-8 px-4 mx-auto max-w-screen-xl text-center lg:py-16 mt-30"
        >
          <h1
            class="gradient-text mb-2 mt-10 my-10 text-4xl font-extrabold tracking-tight leading-none text-gray-900 md:text-5xl lg:text-6xl dark:text-white py-10"
          >
            the randomizer is searching
          </h1>
          <p
            class="mb-8 text-lg font-normal text-gray-500 lg:text-2xl sm:px-16 lg:px-48 dark:text-gray-400"
          >
            this will probably take a while
          </p>
          <div
            class="flex flex-col space-y-4 sm:flex-row sm:justify-center sm:space-y-0 text-gray-500"
          >
            checked <kbd
              id="videoidfailcount"
              class="mx-2 px-1 bg-black text-gray-400 font-mono rounded">3</kbd
            > video ids.
          </div>
          <div
            class="flex flex-col space-y-4 sm:flex-row sm:justify-center sm:space-y-0 text-gray-500"
          >
            if video ids stop going up, press the reroll button
            <br />
            you may need to click it once to get it started
          </div>
          <button
            class="select-none
          rounded-lg bg-gray-600
          py-2 px-2
          text-center align-middle font-sans
          text-white
          shadow-md shadow-gray-900/10
          transition-all hover:shadow-lg
          hover:shadow-gray-900/20
          active:opacity-[0.85]
          disabled:pointer-events-none
          disabled:opacity-50
          disabled:shadow-none
          text-lg
          "
            on:click={loadYT}
            type="button"
          >
            reroll
          </button>
          <p
            class="mt-10 text-lg font-normal text-gray-500 lg:text-xl sm:px-16 lg:px-48 dark:text-gray-400"
          >
            turn off true random below to go back to curated mode.
          </p>
        </div>
      </section>
    {:else}
      <div id="player" class="mt-3 z-1"></div>
    {/if}
  </div>
  <label
    for="randomizer-switch"
    class="inline-flex items-center cursor-pointer mt-1"
  >
    <input
      type="checkbox"
      value=""
      class="sr-only peer"
      id="randomizer-switch"
      on:change={toggleTrueRandom}
    />
    <div
      class="
      relative 
      w-9 
      h-5 
      bg-gray-200 
      peer-focus:outline-none 
      peer-focus:ring-4 
      peer-focus:ring-blue-300 
      dark:peer-focus:ring-blue-800 
      rounded-full peer 
      dark:bg-gray-700 
      peer-checked:after:translate-x-full 
      rtl:peer-checked:after:-translate-x-full 
      peer-checked:after:border-white 
      after:content-[''] after:absolute 
      after:top-[2px] after:start-[2px] 
      after:bg-white 
      after:border-gray-300 
      after:border 
      after:rounded-full 
      after:h-4 after:w-4 
      after:transition-all 
      dark:border-gray-600 
      peer-checked:bg-gradient-to-tr from-indigo-500 from-10% via-sky-500 via-30% to-green-500"
    ></div>
    <span class="ms-3 text-xs font-medium text-gray-600 "
      >true random </span
    >
  </label>
</div>
