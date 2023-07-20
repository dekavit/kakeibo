<script>
  import { spring, tweened } from "svelte/motion";
  import Pie from "../../Pie.svelte";

  let formName, formVal;
  let items = [];
  let percent = 0,
    percent1 = 0,
    percent2 = 0,
    percent3 = 0;
  let maxMoney = 100000;

  let sumMoney = 0,
    sumMoney1 = 0,
    sumMoney2 = 0,
    sumMoney3 = 0;

  const store = tweened(0, { duration: 400 });
  $: store.set(percent);
  const store1 = tweened(0, { duration: 400 });
  $: store1.set(percent1);
  const store2 = tweened(0, { duration: 400 });
  $: store2.set(percent2);
  const store3 = tweened(0, { duration: 400 });
  $: store3.set(percent3);

  let questions = [
    { id: 0, text: "その他" },
    { id: 1, text: `ゲーム` },
    { id: 2, text: `本` },
    { id: 3, text: `食費` },
  ];

  let selected = null;

  let answer = "";

  function handleSubmit() {
    alert(
      `answered question ${selected.id} (${selected.text}) with "${answer}"`
    );
  }

  const data = [
    { label: "項目1", value: 30 },
    { label: "項目2", value: 20 },
    { label: "項目3", value: 50 },
  ];
</script>

<head>
  <meta charset="utf-8" />
  <title>家計簿</title>
  <link rel="stylesheet" href="style.css" />

  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=M+PLUS+Rounded+1c:wght@100;300;900&display=swap"
    rel="stylesheet"
  />
</head>

<main>
  <body>
    <header>
      <h1>家計簿</h1>
    </header>

    <div class="unko">
      <h2>支出</h2>
    </div>

    <div class="ks">
      <Pie
        size={200}
        percent={$store}
        percent1={$store1}
        percent2={$store2}
        percent3={$store3}
      />
      <table id="table_edit" width="500">
        <tr>
          <td>番号</td>
          <td>品目名</td>
          <td>金額(単位 円)</td>
          <td>タグ </td>
          <td>削除</td>
        </tr>
        {#each items as [name, val, tag], i}
          <tr>
            <td>{i}</td>
            <td contenteditable="true">{name}</td>
            <td id="editor" contenteditable="true">{val}</td>
            <td>{questions[tag].text}</td>
            <td>
              <button
                class="deleteButton"
                on:click={() => {
                  items = items.slice(0, i).concat(items.slice(i + 1));
                  sumMoney = items.reduce((a, b) => a + b[1], 0);
                  $: store.set((sumMoney / maxMoney) * 100);
                }}>🔥</button
              >
            </td>
          </tr>
        {/each}
        <tr style="">
          <th colspan="2">計</th>
          <td>{sumMoney}</td>
          <td />
        </tr>
      </table>

      <script>
        editor.addEventListener("charge", function () {
          console.log("aaa");
        });
      </script>

      <br />
      <form
        on:submit|preventDefault={() => {
          if (!formName || !formVal) {
            alert("有効な値を入れてください。💩");
            return;
          }
          items = [...items, [formName, formVal, selected ? selected.id : 0]];
          sumMoney = items.reduce((a, b) => a + b[1], 0);
          if (selected != null) {
            if (selected.id >= 1) {
              sumMoney1 += formVal;
            }
            if (selected.id >= 2) {
              sumMoney2 += formVal;
            }
            if (selected.id >= 3) {
              sumMoney3 += formVal;
            }
          }
          formName = formVal = selected = null;
          $: store.set((sumMoney / maxMoney) * 100);
          $: store1.set((sumMoney1 / maxMoney) * 100);
          $: store2.set((sumMoney2 / maxMoney) * 100);
          $: store3.set((sumMoney3 / maxMoney) * 100);
        }}
      >
        <input placeholder="項目名" bind:value={formName} />
        <input type="number" placeholder="金額を入力" bind:value={formVal} />
        <select bind:value={selected} on:change={() => (answer = "")}>
          {#each questions as question}
            <option value={question}>
              {question.text}
            </option>
          {/each}
        </select>
        <button type="submit">品目の追加</button>
      </form>
    </div>
  </body>
</main>

<style>
  /* main {
      text-align: center;
      padding: 1em;
      max-width: 240px;
      margin: 0 auto;
    } */

  /*///////////////////*/
  * {
    font-family: "M PLUS Rounded 1c", sans-serif;
  }

  header {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background: #ffffff;
    box-sizing: border-box;
  }

  h1 {
    text-align: center; /* テキストを中央揃えに */
    font-size: 3em;
    background-color: #ffb6c1;
  }

  h2 {
    text-align: center;
    font-size: 2em;
    color: #c71585;
  }
  h3 {
    text-align: center; /* テキストを中央揃えに */
    font-size: 3em;
    color: #c71585;
  }

  .unko {
    padding-top: 150px;
    display: flex;
    text-align: center; /* テキストを中央揃えに */
    animation: fadein 3s forwards;
  }
  @keyframes fadein {
    0% {
      opacity: 0;
    }

    100% {
      opacity: 1;
    }
  }
  .unko > div {
    width: 50%;
    text-align: center; /* テキストを中央揃えに */
  }

  .ks > div {
  }

  button {
    min-width: 100px;
    font-family: "M PLUS Rounded 1c", sans-serif;
    appearance: none;
    border: 1;
    border-radius: 8px;
    background: #ffffff;
    color: #000000;
    padding: 8px 16px;
    font-size: 20px;
    cursor: pointer;
  }

  button:hover {
    background: #b3b3b3;
  }

  button:focus {
    outline: none;
    box-shadow: 0 0 0 2px #cbcbcb;
  }

  .googleCalendar iframe {
    width: 75%;
    height: 400px;
  }
  /*///////////////////*/
  .deleteButton {
    border: 0;
    background: yellow; /* 後でスタイルいじる */
  }
  /* 
    @media (min-width: 640px) {
      main {
        max-width: none;
      }
    }
    #table_edit {
      border-collapse: collapse;
    }
    #table_edit td {
      border: 1px solid lightgray;
    } */
</style>
