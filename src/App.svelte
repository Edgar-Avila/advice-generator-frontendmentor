<script>
  const getAdvice = async () => {
    const res = await fetch("https://api.adviceslip.com/advice");
    const data = await res.json();
    if (res.ok) {
      return {
        id: data.slip.id,
        advice: data.slip.advice,
      };
    }
    throw new Error(data);
  };

  let promise = getAdvice();
</script>

<main>
  <div class="card">
    {#await promise}
      <p>Waiting...</p>
    {:then { id, advice }}
      <h3>ADVICE #{id}</h3>
      <p>"{advice}"</p>
    {:catch err}
      <p>{err}</p>
    {/await}
    <img src="pattern-divider-desktop.svg" alt="divider" class="divider">
    <button on:click={() => {
      promise = getAdvice();
    }}>
      <img src="icon-dice.svg" alt="random">
    </button>
  </div>
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;800&display=swap');
  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Manrope", sans-serif;
  }

  :root {
    --light-cyan: hsl(193, 38%, 86%);
    --neon-green: hsl(150, 100%, 66%);
    --grayish-blue: hsl(217, 19%, 38%);
    --dark-grayish-blue: hsl(217, 19%, 24%);
    --dark-blue: hsl(218, 23%, 16%);
  }
  main {
    background-color: var(--dark-blue);
    height: 100vh;
    display: grid;
    place-content: center;
  }
  .card {
    position: relative;
    background-color: var(--dark-grayish-blue);
    max-width: 400px;
    padding: 2em;
    border-radius: 10px;
  }
  .card h3 {
    color: var(--neon-green);
    font-weight: 400;
    font-size: 0.7rem;
    text-align: center;
    letter-spacing: 3px;
  }
  .card p {
    text-align: center;
    color: var(--light-cyan);
    font-weight: 800;
    font-size: 1.2rem;
    padding: 1em 0;
  }
  .card .divider {
    width: 100%;
    margin-bottom: 1em;
  }
  .card button {
    border: none;
    display: grid;
    place-items: center;
    position: absolute;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background-color: var(--neon-green);
    bottom: -25px;
    left: calc(50% - 25px);
    cursor: pointer;
  }
  .card button:hover {
    box-shadow: 0 0 20px 0px var(--neon-green);
  }
  .card button img {
    width: 40%;
  }
</style>
