<div id="stellar-cortex-demo">
  <div id="stellar-cortex-demo-loader">
    <h3>Click to load demo ({{ size }}MiB)</h3>
  </div>
  <script type="module">
    const demo = document.getElementById("stellar-cortex-demo");
    const loader = document.getElementById("stellar-cortex-demo-loader");
    demo.addEventListener("click", () => {
      demo.removeChild(loader);
      const canvas = document.createElement("canvas");
      canvas.id = "stellar-cortex";
      demo.appendChild(canvas);
      const script = document.createElement("script");
      script.type = "module";
      script.innerText = "import init from \"/stellar-cortex/demos/{{ id }}/game.js\"; init();"
      demo.appendChild(script);
    }, { once: true });
  </script>
</div>
