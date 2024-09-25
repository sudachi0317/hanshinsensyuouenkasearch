# hanshinsensyuouenkasearch
<!DOCTYPE html> <html lang="ja"> <head> <meta charset="UTF-8"> <meta name="viewport" content="width=device-width, initial-scale=1.0"> <title>野球用語検索サイト</title> <style> body {
  font-family: Arial,
  sans-serif;
  margin: 0;
  padding: 20px;
  background-color: #f9f9f9;
} h1 {
  font-size: 24px;
  text-align: center;
} input [ type="text" ] {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px
  solid #ccc;
  box-sizing: border-box;
} button {
  padding: 10px;
  background-color: #fff700;
  color: white;
  border: none;
  cursor: pointer;
  width: 100%;
} button:hover {
  background-color: #fff700;
} #results {
  margin-top: 20px;
  padding: 10px;
  background-color: #fff;
  border: 1px
  solid #ccc;
} </style> </head> <body> <h1>阪神タイガース 個別選手応援歌検索サイト</h1> <p>阪神タイガース 個別選手応援歌を検索できるサイトです。<br>選手の背番号を入力するとその選手の応援歌を検索できます。</br></p><input type="text" id="searchTerm" placeholder="検索したい用語を入力"> <button onclick="searchTerm()">検索</button> <div id="results"></div> <script>
// 検索用語のデータ
const terms = {
  "1": "森下 翔太 Shota Morishita<br>夢つかめ 豪快なパワー 渾身のフルスイング 翔けろ 森下 かっとばせー もーりーしたー",
  "0": "木浪 聖也 Seiya Kinami<br>熱き心 乗せたスイング 行（ゆ）くぞ 木浪 勝利目指せ かっとばせー キーなーみー",
  "2": "梅野 隆太郎 Ryutarou umeno<br>気迫のスイング　燃えろよ梅野 どでかいアーチ　勝利への一撃 かっとばせー うーめーの",
  "3": "大山 悠輔 Yusuke Oyama<br>悠然と振り構えたバットに 我らの夢を乗せて我らの夢を乗せて スタンドへはじき返せ 栄光つかむ　その日まで かっとばせー　おーおーやまー<br><br>(専用チャンスマーチ)</br> 【※2回】Oh～Wo Wo Wo～・・・ </br>悠然と振り構えた　バットに我らの夢を乗せて スタンドへはじき返せ　栄光つかむ　その日まで かっとばせ―　おーおーやまー</br>さあここで見せてくれ （オィ オィ オィオィ) さあ主砲の一打を （オィ オィ オィオィオィ） 勝利に導け　我らの大山</br>",
  "5": "近本 光司 Koji Chikamoto<br>切り拓け　勝利への道切り拓け　勝利への道 打て　グラウンド駆けろ　燃えろ　近本 かっとばせー　ちーかーもとー",
  "7": "S.ノイジー Sheldon Neuse<br>ラララ　レッツゴー　シェルドン ラララ　レッツゴー　シェルドン Go to ビクトリー　シェルドン　ノイジー<br>豪打炸裂　飛ばせシェルドン 勝利導け　Go to ビクトリー シェルドン　シェルドン",
  "8": "佐藤 輝明 Teruaki Sato<br>突き進め　さあ行（ゆ）こう　勝利に向かって 振り抜け　輝け　打て　輝明（てるあき かっとばせー　てーるー",
  "12": "坂本 誠志郎 Seishiro Sakamoto<br>狙いを定め　強気で攻めろ お前の時代を築け　さぁぶちかませ かっとばせー　さーかーもとーかっとばせー　さーかーもとー",
  "25": "渡邉 諒 Ryo Watanabe<br>今見せろその打棒　唸りを上げるスイング 打ち抜け　渡邉諒　強打の一撃 かっとばせー　りょーうー",
  "33": "糸原 健斗 Kento Itohara<br>誰にも劣らぬ　ハートの強い男 我武者羅に走り抜け　果敢な戦士 いーとーはらー　いーとーはらー　いーとーはらー 凛々しく　泥臭く　斬り込め全力で 気力込めて　今ここで　燃えさかれ かっとばせー　いーとーはらー",
  "38": "小幡 竜平 Ryuhei Obata<br>光る技を　今こそ見せろ 狙い定めて 小幡　フィールド駆けろ かっとばせー　おーばーたー",
  "51": "中野 拓夢 Takumu Nakano<br>強い気持ちで　勝利を目指せ中野 さあ夢を拓け　打て走れ中野 さあ夢を拓け　打て走れ中野 なーかーのー　なーかーのー",
  "53": "島田 海史 Kairi Shimada<br>さあ行(ゆ)け快足で　チャンスをつかめ 島田海吏　そのスピードで　勝利へ駆けろ しーまーだー　しーまーだー",
  "55": "ヨハン・ミエセス Johan Mieses<br>【×2回】ラーラーラー<br>打球は一直線　スタンド突き刺さる ホームラン　ミエセス　ララララーラーラー</br>【×2回】バモス！　ヨハン！　ミエセス！</br>",
  "58": "前川 右京 Ukyo Maegawa<br>勝利を導く　気迫の一撃 今この瞬間(とき)　振り抜け右京 かっとばせー　うーきょーうーかっとばせー　うーきょーうー",
  "60": "小野寺 暖 Dan Onodera<br>気合いと度胸　努力の証 情熱を燃やして　夢を勝ち取れ ダダン ダン ダダン「おのでらー だん!」 ダダン ダン ダダン「おのでらー だん!」"

  // 必要に応じて他の用語を追加


}; function searchTerm() {
  const term
  = document.getElementById('searchTerm').value.toLowerCase();
  const results
  = Object.keys(terms).filter(key
  => key.toLowerCase().includes(term));
  const output
  = results.length
  ? results.map(key
  => `<strong>$
  { key }
  </strong>: $
  { terms [ key ] }
  `).join('<br>'): "用語が見つかりませんでした。"
  ; document.getElementById('results').innerHTML
  = output;
} </script> </body> </html>
