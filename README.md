# scrape-ipa-and-meaning-wiktionary
An anki script that will scrape wiktionary for the meaning and prounciation of a word

paste the following code on the back side of the card

```javascript
<div id="morph">{{MorphMan_FocusMorph}}</div>
Meaning:
<div id="meaning">no meaning</div>
IPA: 
<div id="ipa">no IPA</div>
<script>
async function get() {
try{
let word = document.getElementById("morph").innerHTML
let res = await fetch(`https://de.wiktionary.org/w/api.php?action=parse&format=json&prop=text%7Crevid%7Cdisplaytitle&origin=*&callback=?&page=${word}`, {
header: {'Content-type': 'UTF-8'}});

let data = await res.text()
data = data.slice(5,data.length-1)
data = JSON.parse(data).parse.text["*"]
let ConvertStringToHTML = function (str) {
   let parser = new DOMParser();
   let doc = parser.parseFromString(str, 'text/html');
   return doc.body;
};
let html = ConvertStringToHTML(data)
let meaning = html.querySelector(`[title = "Sinn und Bezeichnetes (Semantik)"]`).nextElementSibling
let reading = html.querySelector(".ipa")




document.getElementById("meaning").innerHTML = meaning.innerHTML
document.getElementById("ipa").innerHTML = reading.innerHTML
}
catch(err) {
console.log(err)
}
}
get()
</script>
```
