<title>Teshan's stuff</title>

<body>
  <div class="welcome"><span class="blink">\></span>Hola!</div>
  <div class="window"></div>
</body>

<style>
  .welcome {
    text-align: left;
    font-family: monospace;
    font-size: 2.5em;
  }

  .blink {
    animation: blink-animation 1s steps(1, end) infinite;
  }

  @keyframes blink-animation {
    0% { opacity: 1; }
    50% { opacity: 0; }
    100% { opacity: 1; }
  }

  .window {
    width: 70%;
    height: 60%;
    background-color: black;
    position: absolute;
    left: 15%;
    top: 20%;
    border-radius: 20px;
    box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.5);
  }

  body {
    margin: 0;
    padding: 0;
    border-radius: 5px;
    background: linear-gradient(135deg, #6a11cb, #c608df); /* Blue/Purple gradient */
  }
</style>
