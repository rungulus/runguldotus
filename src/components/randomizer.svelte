<script>
    //randomizer 3.0
    //lets use that youtube api
  
    // make a function to load the youtube api
    // this way we can lazy load it
    document.getElementById("placeholder").addEventListener("click", function () {
      loadYT();
    });
    function loadYT() {
      var tag = document.createElement("script");
      tag.src = "https://www.youtube.com/iframe_api";
      var firstScriptTag = document.getElementsByTagName("script")[0];
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
    }
    // then, create a youtube player
    var player;
    function onYouTubeIframeAPIReady() {
      player = new YT.Player("player", {
        height: "270",
        width: "480",
        videoId: randURLs[Math.floor(Math.random() * randURLs.length)],
        playerVars: {
          playsinline: 1,
        },
        events: {
          onReady: onPlayerReady,
          onStateChange: onPlayerStateChange,
        },
      });
    }
  
    // once it's ready, try to play the video automatically
    // this probably won't work in most cases, browsers dont like video playing unattended
    function onPlayerReady(event) {
      event.target.playVideo();
    }
    //check if the video ends, and if so wait 1.5 seconds and re-roll
    function onPlayerStateChange(event) {
      if (event.data == YT.PlayerState.ENDED) {
        setTimeout(reroll, 1500);
      }
    }
    //simply randomly select from the randURLs array
    function reroll() {
      randomvideo = randURLs[Math.floor(Math.random() * randURLs.length)];
      console.log("the randomly selected video id is " + randomvideo);
      player.loadVideoById(randomvideo);
    }
    var randURLs = [
      "fCmvXCgZr74",
      "fCmvXCgZr74",
      "4ZQRHNG1IYE",
      "_0AlytWWDm",
      "2REG3-Wb5gM",
      "2X5hyC62zZ4",
      "Cb6F14AGrvI",
      "4DUkS98yw5Y",
      "vl_aWoPNpJM",
      "DOWO4gq-whg",
      "hxpbJ89pHAM",
      "A7qXoSgYR4c",
      "bxRzsgtvakY",
      "Epd4pUxkogw",
      "ag46jiqISO4",
      "_xyGvpLOZ1Q",
      "WFoC3TR5rzI",
      "juQyy00x-Mw",
      "VfCYZ3pks48",
      "Xh-71XzC8pw",
      "R37bwKO9gBA",
      "dQw4w9WgXcQ",
      "ZZ5LpwO-An4",
      "Li4j82QbBvk",
      "VqB1uoDTdKM",
      "QrGrOK8oZG8",
      "Ivgx5x1dob8",
      "IfatKHskzZc",
      "EY2kBBXi0XY",
      "rICeCUzaY-g",
      "1Ceh-TkR75g",
      "yOFhvi68KW0",
      "UO2sYTkFB0k",
      "JGS4AVRtwr8",
      "wBl45GPoGHY",
      "iBQRjiAHUX8",
      "e1bEWoblGZA",
      "MMY_SUuobww",
      "hd2uvPjMRy4",
      "dNOztYZdT6A",
      "mdsS7dLR-d8",
      "3QyMXAu-kt0",
      "kDJs-yV25sA",
      "hqCbx5LQIlw",
      "McQ58nhhpHM",
      "coCNTFSjd_M",
      "t4Aio5BGleA",
      "iThGJXTucxY",
      "AFalBiA5Tf8",
      "yeNEnvn3JCE",
      "IqPQZFvO2yM",
      "UxBAY4nfRBU",
      "EF8GhC-T_Mo",
      "kpk2tdsPh0A",
      "pA2vWc2eeNE",
      "pYcpZJVmXvU",
      "bZe5J8SVCYQ",
      "cJtrHIMmMy4",
      "xxprbbytUQI",
      "XKBwItDNrEU",
      "dKx1wnXClcI",
      "VyvsXRBHwTA",
      "2Qu3hZnvfGg",
      "KgtRRBtSjuo",
      "0tDz_B-PFpg",
      "iUJ8r4ReHog",
      "D1Uhk-QN11M",
      "5tFZof9EDhc",
      "EShUeudtaFg",
      "_-Rji3mip4A",
      "z5qLgL-E7uY",
      "_B13yISVHWI",
      "Q0K_zvlDzmI",
      "o7SdP-gnfIc",
      "T-w0h3g07aE",
      "pC6HZPJH6wE",
      "BMBcZwoGPw4",
      "_zzEDrYTkkg",
      "1EZ9XJssiqY",
      "qlIlHCjrM_E",
      "cU0oHq9Y6Ws",
      "gyW8_MEG6tE",
      "l2QClcUUlhc",
      "0MIH948b_uQ",
      "W_JhpXm5e4g",
      "0HHh-stDn5k",
      "DkYQ72lGrew",
      "_EJsR8mgVWk",
      "Rc-Jh3Oe0Gc",
      "5NHaO0NVbAg",
      "j-GnuHwK8mw",
      "h7_7w_wL64g",
      "4ISDUKG-N90",
      "BX2PRxXQbXE",
      "2okrgAMdHok",
      "mJYAvNvve0E",
      "fV3nflAQ99w",
      "t4mGPIWbw0c",
      "0mQCi4LQUQw",
      "FG7eYo_ozJQ",
      "heEKi5EjZXA",
      "xt2OdUfYZbk",
      "0E_5SynW3aM",
      "0iUTZajxlWI",
      "bLHL75H_VEM",
      "UddCTvioEnw",
      "ixWybGQnPHQ",
      "v11SYt_brN8",
      "hnsNMoor5Ic",
      "csFf8EiCQ2Y",
      "d1CRGkDgrrQ",
      "GjjZacZSWT4",
      "NYdU1p7kDxY",
      "6f5ys8VU2ek",
      "MDM304JW_NA",
      "UfNRWh4s_ag",
      "4ONHOwTBbNw",
      "oNjD7ogr--g",
      "YKAPZgtYzIs",
      "LYSPaCiKpQw",
      "_oVm2jtv6q8",
      "HKVoN-SjZ-Q",
      "6dfM7Hiah4s",
      "7FiiEZ9Z3aQ",
      "IanEey1mEIg",
      "qCIlcC6c1i8",
      "SyzM3yjkQko",
      "mJUvIhI_jpo",
      "z94lg7dGkw4",
      "h6OKgREcHFM",
      "E7w_v9J7iOc",
      "0UR3bAfuNyg",
      "SoZ41i2dSIw",
      "-sk9kXyfGvU",
      "dvOjGkfea_w",
      "bmFGbBmlyKQ",
      "IxTAJNifDAI",
      "NnDe2u5M7e8",
      "y7wyfTsIm1k",
      "T-VZ1kL8ZgE",
      "Udk8YHOy0EU",
      "wbfu39l0kxg",
      "3fG8rNHUspU",
      "FOtgipv4oa8",
      "VDP6RCJSsMg",
      "etfsKeog6S4",
      "YG8vtggc1Ok",
      "RBGW9J3sx1g",
      "VCNskcGD1Oc",
      "OBEiYEk3zD4",
      "QBK3QpQVnaw",
      "0tdyU_gW6WE",
      "VqB1uoDTdKM",
      "SNvDUO42Hys",
      "j13HG2HiEAk",
      "sdmsRcsl_xA",
    ];
  </script>