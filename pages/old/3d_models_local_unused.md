
<!-- Import the component -->
<script type="module" src="https://ajax.googleapis.com/ajax/libs/model-viewer/4.0.0/model-viewer.min.js"></script>

<style>
  /* Vertical layout container */
  .model-grid {
    display: grid;
     /* Create as many columns as fit, each at least 300px wide */
    grid-template-columns: repeat(auto-fit, minmax(45%, 1fr));
    /* High gap for vertical and horizontal spacing */
    gap: 60px 15px; 
    padding: 15px;
    width: 100%;
    max-width: 1400px; /* Optional: caps the grid width on huge monitors */
    margin: 0 auto;    /* Centers the whole grid */
    box-sizing: border-box;
    align-items: top; 
  }

  /* Frame for each model + text */
  .model-frame {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    /* Forces the entire frame to be a specific shape */
    aspect-ratio: 2/1; 
  }

  model-viewer {
    width: 100%;
    flex-grow: 1;
    height: 0;
    /*aspect-ratio: 2/1;*/
    background-color: #f5f5f5;
    border: 1px solid #ddd;
    border-radius: 8px;
    overflow: hidden;
    position: relative;
  }

    /* Container for the poster image and button */
  .custom-poster {
    position: absolute;
    left: 0; right: 0; top: 0; bottom: 0;
    background-size: contain;
    background-position: center;
    background-repeat: no-repeat;
    display: flex;
    justify-content: center;
    align-items: center;
  }

    /* load button */
  .load-btn {
    padding: 14px 30px;
    background: #000;
    color: #fff;
    border: none;
    border-radius: 15px;
    cursor: pointer;
    font-size: 1rem;
    font-weight: 600;
    
    /* Use transform for hover so the button doesn't move/shift */
    transition: transform 0.2s cubic-bezier(0.4, 0, 0.2, 1), background-color 0.2s;
    box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    
    /* Prevents text selection during rapid clicks */
    user-select: none; 
  }

  .load-btn:hover {
    transform: scale(1.05); /* Scales up without moving the button's center point */
    background-color: #333;
  }

  .load-btn:active {
    transform: scale(0.98); /* Slight press effect */
  }

    .model-text {
    margin-top: 20px;
    font-family: sans-serif;
    color: #333;
    text-align: center;
    max-width: 80%;
    line-height: 1.5;
  }

  /* Style for the fullscreen toggle button */
  .fullscreen-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: 8px;
    background: rgba(255, 255, 255, 0.8);
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
    z-index: 100;
  }

  /* Optional: Hide button while poster is visible */
  model-viewer[poster-visible] .fullscreen-btn {
    display: none;
  }

</style>

<div class="model-grid">

  <!-- Model 1 -->
  <div class="model-frame">
    <model-viewer id="mv1" src="../../assets/obj/atelier_janelao.glb" reveal="manual" camera-controls tone-mapping="neutral" shadow-intensity="1.87" exposure="0.52" camera-orbit="0deg 67.48deg 45m" field-of-view="24.96deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv1').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv1')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>atelier_janelao</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>


  <!-- Model 2 -->
  <div class="model-frame">
    <model-viewer id="mv2" src="../../assets/obj/lapa_do_caboclo.glb" reveal="manual" camera-controls tone-mapping="neutral" shadow-intensity="1.39" camera-orbit="0deg 75deg 39m" field-of-view="24deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv2').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv2')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>lapa_do_caboclo</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>



  <!-- Model 3 -->
  <div class="model-frame">
    <model-viewer id="mv3" src="../../assets/obj/arco_do_andre_interior.glb" reveal="manual" camera-controls tone-mapping="neutral" shadow-intensity="1.39" camera-orbit="0deg 75deg 42m" field-of-view="24deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv3').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv3')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>arco_do_andre_interior</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>


  <!-- Model 4 -->
  <div class="model-frame">
    <model-viewer id="mv4" src="../../assets/obj/protoceratops.glb" reveal="manual" camera-controls tone-mapping="neutral" poster="poster.webp" shadow-intensity="2" exposure="0.26" camera-orbit="-110.8deg 60.91deg 1048m" field-of-view="30deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv4').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv4')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>protoceratops</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>


  <!-- Model 5 -->
  <div class="model-frame">
    <model-viewer id="mv5" src="../../assets/obj/garopaba_dune_field_dunas_do_siriu.glb" reveal="manual" camera-controls tone-mapping="neutral" poster="poster.webp" shadow-intensity="2" exposure="0.22" camera-orbit="28.17deg 62.94deg 10.69m" field-of-view="20.76deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv5').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv5')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>garopaba_dune_field_dunas_do_siriu</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>



  <!-- Model 6 -->
  <div class="model-frame">
    <model-viewer id="mv6" src="../../assets/obj/garcia_garden_quarry.glb" reveal="manual" camera-controls tone-mapping="neutral" shadow-intensity="1.39" camera-orbit="0deg 75deg 42m" field-of-view="24deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv6').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv6')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>garcia_garden_quarry</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>




  <!-- Model 6 -->
  <div class="model-frame">
    <model-viewer id="mv7" src="../../assets/obj/clastic_dykes_parana_basin.glb" reveal="manual" camera-controls tone-mapping="neutral" shadow-intensity="1.39" camera-orbit="0deg 75deg 42m" field-of-view="24deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv7').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv7')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>clastic_dykes_parana_basin</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>



    <!-- Model 6 -->
  <div class="model-frame">
    <model-viewer id="mv8" src="../../assets/obj/lagoinha_landslide.glb" reveal="manual" camera-controls tone-mapping="neutral" shadow-intensity="1.39" camera-orbit="0deg 75deg 42m" field-of-view="24deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv8').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv8')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>lagoinha_landslide</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>



    <!-- Model 6 -->
  <div class="model-frame">
    <model-viewer id="mv9" src="../../assets/obj/speleothem_devils_cave.glb" reveal="manual" camera-controls tone-mapping="neutral" shadow-intensity="1.39" camera-orbit="0deg 75deg 42m" field-of-view="24deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv9').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv9')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>speleothem_devils_cave</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>



    <!-- Model 6 -->
  <div class="model-frame">
    <model-viewer id="mv10" src="../../assets/obj/caverna_do_diabo_devils_cave.glb" reveal="manual" camera-controls tone-mapping="neutral" shadow-intensity="1.39" camera-orbit="0deg 75deg 42m" field-of-view="24deg">
    <div slot="poster" class="custom-poster" style="background-image: url('../../assets/obj/caboclo_thumb.webp');">
        <button class="load-btn" onclick="document.getElementById('mv10').dismissPoster()">View Model</button>
    </div>
        <!-- Fullscreen Toggle Button -->
    <button class="fullscreen-btn" onclick="toggleFullscreen('mv10')"> ⛶ Fullscreen</button>
    </model-viewer>
  </div>
  <div class="model-frame">
    <div class="model-text">
      <strong>caverna_do_diabo_devils_cave</strong><br>
      High-performance industrial component designed for 2026 specifications.
    </div>
  </div>


</div>




<script>
  function toggleFullscreen(id) {
    const viewer = document.getElementById(id);
    if (!document.fullscreenElement) {
      viewer.requestFullscreen().catch(err => {
        alert(`Error attempting to enable full-screen mode: ${err.message}`);
      });
    } else {
      document.exitFullscreen();
    }
  }
</script>



