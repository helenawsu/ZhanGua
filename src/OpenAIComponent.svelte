<!-- OpenAIComponent.svelte -->
<script>
  import {randomKaomoji} from "./randomKaomoji.js"
  import {hexagramNames} from "./hexagramInfo.js";
    const apiKey = process.env.API_KEY;
    let prompt = '';
    let response = '';
    let reply = '';
    let randum = 0;
    let hemoji = "";
    let hexName = "";
    function getRandomNumber() {
      return Math.floor(Math.random() * (67 - 4 + 1)) + 4; // min 4 max 67
    }
    async function callOpenAI() {
      
    randum = getRandomNumber();
    hemoji = String.fromCharCode(randum + 19900);
    hexName = hexagramNames[randum - 4];
      try {
        console.log("starting api call");
        const apiUrl = 'https://api.openai.com/v1/chat/completions';
        const startMessage = 'You will act like a expert Yijing prophet. You will give prediction or advice of an event based on Yijing knowledge. Use the hexagram' +hexName+ 'and explain how it is related to the event. If the event can not be advised by Yijing or you cannot understand the input, tell them to take this seriously.';
        const requestBody = {
          model: 'gpt-3.5-turbo',
          messages: [
          { role: 'system', content: startMessage },
          { role: 'user', content: prompt },
        ],
          max_tokens: 1500, // Adjust as needed
        };
  
        let response = await fetch(apiUrl, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${apiKey}`,
          },
          body: JSON.stringify(requestBody),
        });        
        console.log("ending api call");
        const data = await response.json();
        // Handle the response as needed
        console.log(data);
        response = data.choices[0].message.content;
        console.log(response);
        reply = JSON.stringify(response).replace(/\n/g, '<br />');
        console.log(reply);
      } catch (error) {
        console.error('Error', error);
      }
    }
  </script>
  
  <main>
    <textarea bind:value={prompt} placeholder=""></textarea>
    <button on:click={callOpenAI}>submit</button>
    {#if {response}}
      <div>
        <h1>{hemoji}</h1>
        <h2>{hexName}</h2>
        <p>{reply}</p>
      </div>
    {/if}
  </main>
  
  <style>
    main {
      text-align: center;
      margin: 2rem;
    }
  
    textarea {
      width: 100%;
      margin-bottom: 1rem;
    }
    h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }
  </style>
  