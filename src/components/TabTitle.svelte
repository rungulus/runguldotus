<script>
    //silly script to change the title of the webpage when the tab is no longer focused
  
    // store the original tab title
    var origTitle = document.title;
  
    // function to change title when focusing on tab
    function oldTitle() {
      document.title = origTitle;
    }
  
    // function to change title when un-focusing on tab
    function newTitle() {
      document.title = "🥺 Come back!";
    }
  
    // bind functions to blur and focus events
    window.onblur = newTitle;
    window.onfocus = oldTitle;
  </script>