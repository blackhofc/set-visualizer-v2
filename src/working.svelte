<script>
  import { onMount } from "svelte";

  let numbers = [];
  let newNumber = "";
  let weightPerDisk = 12.5; // Peso por disco inicial

  function createSVGElements(weight, discWeight) {
    const N = weight / discWeight;
    let squares = [];
    let sideSquareWidth = 20; // (20 * 1) / (N / 2);
    let sideSquareHeight = 50;
    let middleSquareWidth = N * sideSquareWidth;
    let middleSquareHeight = sideSquareHeight / 4;

    for (let i = 0; i < N / 2; i++) {
      squares.push({ id: i });
    }

    return {
      squares,
      sideSquareWidth,
      sideSquareHeight,
      middleSquareWidth,
      middleSquareHeight,
    };
  }

  function addNumber(num) {
    numbers = [...numbers, num];
    console.log(numbers);
  }

  onMount(() => {
    // Llamar a la función con el número par deseado
    // addNumber({ weight: initialWeight, numDisks: 2 });
  });

  function handleKeyDown(event) {
    if (event.key === "Enter") {
      if (parseInt(newNumber, 10) > 488) {
        alert("El récord de levantamiento es de 488 kg.");
        newNumber = "";
        return;
      }
      addNumber({
        weight:
          Math.round(parseInt(newNumber, 10) / weightPerDisk) * weightPerDisk,
        discWeight: weightPerDisk,
      });
      newNumber = "";
    }
  }
</script>

<div class="title">Visualización de pesa</div>

<div class="container">
  {#each numbers as { weight, discWeight }}
    <div class="card">
      {#each createSVGElements(weight, discWeight).squares as square}
        <svg
          class="side-square"
          width={createSVGElements(weight, discWeight).sideSquareWidth}
          height={createSVGElements(weight, discWeight).sideSquareHeight}
        >
          <rect
            width={createSVGElements(weight, discWeight).sideSquareWidth}
            height={createSVGElements(weight, discWeight).sideSquareHeight}
            fill="#252525"
          />
        </svg>
      {/each}
      <svg
        width={createSVGElements(weight, discWeight).middleSquareWidth < 200
          ? createSVGElements(weight, discWeight).middleSquareWidth
          : 200}
        height={createSVGElements(weight, discWeight).middleSquareHeight}
      >
        <rect
          x="0"
          y="0"
          width={createSVGElements(weight, discWeight).middleSquareWidth}
          height={createSVGElements(weight, discWeight).sideSquareHeight}
          fill="#252525"
        />
      </svg>
      {#each createSVGElements(weight, discWeight).squares as square}
        <svg
          class="side-square"
          width={createSVGElements(weight, discWeight).sideSquareWidth}
          height={createSVGElements(weight, discWeight).sideSquareHeight}
        >
          <rect
            width={createSVGElements(weight, discWeight).sideSquareWidth}
            height={createSVGElements(weight, discWeight).sideSquareHeight}
            fill="#252525"
          />
        </svg>
      {/each}
      <div class="number-text">{weight} kg en Discos de {discWeight} kg</div>
    </div>
  {/each}
</div>

<div class="input-container">
  <input
    type="number"
    bind:value={newNumber}
    min="1"
    placeholder="Ingresa el peso total"
    on:keydown={handleKeyDown}
  />
  <input
    type="number"
    bind:value={weightPerDisk}
    min="1"
    placeholder="Peso por disco"
  />
</div>

<style>
  .title {
    text-align: center;
    font-size: 24px;
    margin-bottom: 20px;
  }

  .container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: auto;
    margin-top: 20px;
  }

  .card {
    position: relative;
    margin: 10px;
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 10px;
    height: auto; /* Para que la tarjeta se ajuste a la altura del contenido */
  }

  .side-square {
    margin: 0 2px; /* Ajusta el margen entre los cuadraditos según necesites */
    border-radius: 3px;
  }

  .input-container {
    text-align: center;
    margin-top: 10px;
    margin-bottom: 20px;
    margin-left: auto; /* Centra el contenedor horizontalmente */
    margin-right: auto;
  }

  .number-text {
    position: absolute;
    bottom: 2px; /* Ajusta la distancia desde la parte inferior */
    left: 50%;
    transform: translateX(-50%);
    font-size: 14px;
    font-weight: bold;
    color: #e71964;
  }
</style>
