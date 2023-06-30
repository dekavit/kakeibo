<script>
  import { loop_guard } from "svelte/internal"
  import { spring, tweened } from "svelte/motion"
  import Pie from "./Pie.svelte"

  let formName, formVal
  let items = []
  let percent = 0
  let maxMoney = 100000
  let sumMoney = 0
  const store = tweened(0, { duration: 400 })
  $: store.set(percent)
</script>

<head>
  <meta charset="utf-8" />
  <title>家計簿</title>
  <link rel="stylesheet" href="style.css" />

  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=M+PLUS+Rounded+1c:wght@100;300;900&display=swap"
    rel="stylesheet" />
</head>

<main>
  <body>
    <header>
      <h1>家計簿</h1>
    </header>

    <div class="unko">
      <div>
        <h2>メニュー</h2>
        <button type="“button”" onclick="location.href='syuusi.html'"
          >収支</button>
        <button type="“button”" onclick="location.href='tyokin.html'"
          >貯金</button>
        <br /><br /><br />

        <h2>カレンダー</h2>
        <div class="googleCalendar">
          <iframe
            src="https://calendar.google.com/calendar/embed?height=400&wkst=1&bgcolor=%23ffffff&ctz=Asia%2FTokyo&showTitle=0&title=%E3%82%AB%E3%83%AC%E3%83%B3%E3%83%80%E3%83%BC&showNav=1&showDate=1&showPrint=1&showTabs=1&src=YjdjNTVlZjkyODRmMDI3MGQzODExNjg4NmM3YjY3MWI3ZWExMjliODIzZTE2N2VlMzZlNDRmMjgyZjEwMDgwOUBncm91cC5jYWxlbmRhci5nb29nbGUuY29t&src=amEuamFwYW5lc2UjaG9saWRheUBncm91cC52LmNhbGVuZGFyLmdvb2dsZS5jb20&color=%23E4C441&color=%230B8043"
            title="calendar"
            style="border-width: 0;"
            width="500"
            height="400"
            frameborder="0"
            scrolling="no" />
        </div>
      </div>

      <h2>編集</h2>

      <table id="table_edit" align="center" width="500">
        <tr>
          <td>番号</td>
          <td>品目名</td>
          <td>金額(単位 円)</td>
          <td>削除</td>
        </tr>
        {#each items as [name, val], i}
          <tr>
            <td>{i}</td>
            <td contenteditable="true">{name}</td>
            <td id="editor" contenteditable="true">{val}</td>
            <td>
              <button
                class="deleteButton"
                on:click={() => {
                  items = items.slice(0, i).concat(items.slice(i + 1))
                  sumMoney = items.reduce((a, b) => a + b[1], 0)
                  $: store.set((sumMoney / maxMoney) * 100)
                }}>🔥</button>
            </td>
          </tr>
        {/each}
        <tr style="">
          <th colspan="2">計</th>
          <td>{sumMoney}</td>
          <td />
        </tr>
      </table>

      <!-- <script>
        editor.addEventListener("charge", function () {
          console.log("aaa")
        })
      </script> -->

      <br />
      <form
        on:submit|preventDefault={() => {
          if (!formName || !formVal) {
            alert("有効な値を入れてください。💩")
            return
          }
          items = [...items, [formName, formVal]]
          sumMoney = items.reduce((a, b) => a + b[1], 0)
          formName = formVal = null
          console.log(sumMoney)
          $: store.set((sumMoney / maxMoney) * 100)
        }}>
        <input placeholder="項目名" bind:value={formName} />
        <input type="number" placeholder="金額を入力" bind:value={formVal} />
        <button type="submit">品目の追加</button>
      </form>

      <Pie size={200} percent={$store} />

      <div>
        <h2>シェア</h2>
        <!-- twitter -->
        <a
          href="http://twitter.com/share?url=yurukei-career.com&text=Twitterのシェアをするときの文章です&via=yurukei20&hashtags=ハッシュタグのテキスト"
          target="_blank"
          rel="nofollow noopener noreferrer">Twitterで共有する</a>
        <br /><br /><br />
        <h2>フレンド</h2>
        <br /><br /><br /><br />
        <h3>現在のセルラン相場</h3>
        <button
          type="button"
          onclick="location.href='https://game-i.daa.jp/?AppStore%E3%82%A2%E3%83%97%E3%83%AA%E6%9C%80%E6%96%B0%E3%82%BB%E3%83%BC%E3%83%AB%E3%82%B9%E3%83%A9%E3%83%B3%E3%82%AD%E3%83%B3%E3%82%B0'"
          >チェック</button>
        <br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </div>
    </div>
  </body>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  /* h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  } */

  .deleteButton {
    border: 0;
    background: yellow; /* 後でスタイルいじる */
  }

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
  }
</style>
