<script>
  import { onMount } from "svelte";

  const PLATES = {
    IL: "images/initial-left.png",
    IR: "images/initial-right.png",
    L: "images/weight-left.png",
    R: "images/weight-right.png",
    EL: "images/end-left.png",
    ER: "images/end-right.png",
  };

  let plates = [];
  let weight = "";

  function calculateLeftOffset(index) {
    console.log("index", index);
    return -42.3 - index * 14.3;
  }

  function addPlate(plate) {
    plates = [...plates, plate];
    console.log(plates);
  }

  function initLift() {
    plates = [];

    if (parseInt(weight, 10) > 30000) {
      alert("El récord de levantamiento es de 488 kg.");
      weight = "";
      return;
    }

    const platesCount = Math.round(parseInt(weight, 10) / 25) / 2;
    weight = "";

    for (let i = 0; i < platesCount; i++) {
      const lSrc = i === 0 ? PLATES.IL : PLATES.L;
      const rSrc = i === 0 ? PLATES.IR : PLATES.R;

      addPlate({ lSrc, rSrc, weight: 25 });
    }
  }

  onMount(() => {
    // Cargo la vista
    // initLift();
  });

  function handleKeyDown(event) {
    if (event.key === "Enter") {
      // Ignore enter key event for testing purposes
      initLift();
    }
  }
</script>

<div class="top-left-image-container">
  <img src="images/sign.png" alt="About" class="top-left-image" />
</div>

<div class="input-container">
  <input
    type="number"
    bind:value={weight}
    min="1"
    placeholder="How much you wanna lift?"
    on:keydown={handleKeyDown}
    class="custom-input"
  />
  <button on:click={initLift} class="lift-button">Lift</button>
</div>

<div class="guy-container">
  {#each plates as { lSrc, rSrc, weight }, index}
    <img
      src={lSrc}
      alt="Left weight"
      class="weight-absolute"
      style="left: {calculateLeftOffset(index)}px;"
    />
    <img
      src={rSrc}
      alt="Right weight"
      class="weight-absolute"
      style="right: {calculateLeftOffset(index)}px;"
    />
  {/each}
  {#if plates.length > 0}
    <img src="images/men.png" alt="Descripción de la imagen" />

    <img
      src={PLATES.EL}
      alt="End left weight"
      class="weight-end-absolute"
      style="left: {calculateLeftOffset(plates.length - 1) - 9.5}px;"
    />
    <img
      src={PLATES.ER}
      alt="End right weight"
      class="weight-end-absolute"
      style="right: {calculateLeftOffset(plates.length - 1) - 9.5}px;"
    />
  {:else}
    <img src="images/men-empty.png" alt="Descripción de la imagen" />
  {/if}
</div>

<style>
  :global(body) {
    background-color: black;
    background-image: url("/public/images/background.png");
    height: 100vh;
    width: 100%;
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
  }

  .guy-container {
    position: fixed;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    text-align: center;
  }

  .input-container {
    position: fixed;
    text-align: center;
    top: 100px;
    left: 0;
    right: 0;
    margin-left: auto;
    margin-right: auto;
  }

  .weight-absolute {
    position: absolute;
    bottom: 73.2%;
  }
  .weight-end-absolute {
    position: absolute;
    bottom: 83%;
  }

  @media (max-width: 768px) {
    .weight-absolute {
      display: block;
    }
  }

  .custom-input {
    height: 40px;
    width: 450px;
    color: #f56514;
    text-indent: 6px;
    border: none;
    border-radius: 4px;
  }
  .lift-button {
    width: 100px;
    height: 40px;
    border: none;
    border-radius: 4px;
    background-color: orange;
    color: white;
    font-weight: normal;
  }

  .lift-button:hover {
    background-color: darkorange; /* Cambia el color de fondo al pasar el ratón */
  }

  .top-left-image-container {
    position: fixed;
    top: 0px; /* Adjust as needed */
    left: 50px; /* Adjust the left padding */
    z-index: 9999; /* Adjust the z-index if needed */
  }

  .top-left-image {
    width: 250px; /* Adjust the width of your image */
    height: auto; /* Maintain aspect ratio */
  }
</style>
