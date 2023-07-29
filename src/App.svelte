<script>
  import {hexagramNames} from "./hexagramInfo.js";
  import {randomKaomoji} from "./randomKaomoji.js"

  function getRandomNumber() {
  return Math.floor(Math.random() * (67 - 4 + 1)) + 4; // min 4 max 67
}
  let randum = 0;
  let hemoji = "";
  let hexName = "";
  function randomizeHexagram() {
    if (question.length == 0) {
      hexName = "Please, raise a question."
    } else {
      randum = getRandomNumber();
    hemoji = String.fromCharCode(randum + 19900);
    hexName = hexagramNames[randum - 4];
    }
  }
  /**
   * @type {string}
   */
  let question = "";
</script>

<main>
  <h1>占卦</h1>
  <p>input your question here   <span>{randomKaomoji()}</span>  </p>
  <input bind:value={question}/>
  <button on:click={randomizeHexagram}>submit</button>
  <h1>{hemoji}</h1>
  <h2>{hexName}</h2>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
