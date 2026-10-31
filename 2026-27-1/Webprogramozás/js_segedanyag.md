# 1. rész - TÖMBFÜGGVÉNYEK
## Alapok
- **forEach:** általános ciklus
- **some:** eldöntés
- **every:** optimista eldöntés
- **map:** másolás
- **filter:** kiválogatás
    ```
    const evens = numbers.filter(e =>e % 2 === 0);
- **reduce:** összegzés (sorozatszámítás)
    ``` 
    function sum(x) {
        let s = 0;
        for (const e of x) {
            s = s + e;
        }
        return s;
    }
    // instead
    x.reduce((s, e) => s + e, 0);
- **find:** keresés (elem)
- **findIndex:** keresés (index)





## Példák a mintaZH-ból (elv nagyjából jók)
```
taskA.innerHTML=authors.some(a => a["books"].filter(b=>b["year"]<1900).length>=2)

taskB.innerHTML=authors.every(a=>a["books"].filter(b=>b["genres"].includes("regény")).length>=1)

const au = authors.filter(a=>a["books"].some(b=>b["genres"].includes("novel") && b["pages"]>500)) 
taskC.innerHTML=au.map(a=>a["name"])

taskD.innerHTML=authors.reduce((s,a) => s + a["books"].reduce((z,b) => z+b["pages"],0), 0)

const pagePerAuthor = authors.map(a=>a["books"].reduce((z,b) => z+b["pages"],0))
const maxVal=Math.max(...pagePerAuthor)
const maxInd=pagePerAuthor.findIndex(m => m===maxVal)
taskE.innerHTML=authors[maxInd]["name"]

const bookss=authors.map(a=>a["books"]).flat()
const novelBooks=bookss.filter(b=>b["genres"].includes("novel") && b["year"]<1900).map(b=>b["pages"])
taskF.innerHTML=(novelBooks.reduce((s,e) => s+e, 0) / novelBooks.length)
```

# 2. rész - DOM, ESEMÉNYKEZELÉS
## DOM elem elérése
- ```const elemById = document.querySelector("#id")```
- ```const elemsByClass = document.querySelectorAll(".class")```
```
const pages = document.querySelectorAll('.page');
pages.forEach(p=> p.style=`width: ${pace.value*20}px`)
```

## Value, innerHTML, textContent érdekesek
- ```edition.selectedOptions[0].value``` - optionsnál az adott option értéke

## Gyakori függvények
### String
- ```szoveg.trim()``` - leszedi a whitespacet 
- ```szoveg.toLowerCase()```
- ```szoveg.startsWith(str),endsWith```
- ```charAt(i), charCodeAt(i)``` - keresés elölről, hátulról, indexet ad vissza
- ```indexOf(mit, honnan), lastIndexOf(mit, honnan)```
- ```localeCompare(mivel[, locale])``` - összehasonlítás, -1, 0, 1
- ```search(regexp), replace(mit, mivel), match(regexp), matchAll(regexp)``` - keresés, csere, reguláris kifejezésekkel is
- ```substr(honnan, hossz), substring(mettől, meddig), slice(mettől, meddig)``` - részszöveg
### Tömb
- ```tomb.push(element)```
- ```tomb.unshift(element)```
- ```tomb.shift()```
- ```tomb.includes(element)```
- ```tomb.reverse()```
### HTML
- ```const valami = createElement("li")```
    - ez pl csinál egy listitemet
- ```parentElement.appendChild(childElement)```
    ``` 
    const listItem = document.createElement("li");
    listItem.innerHTML=input;
    readingList.appendChild(listItem);
    //ha létező elemre használjuk ami alapból máshol van, akkor az appendChild eltávolítja az eredeti helyéről! nem csak másolatot készít
    ```
- ```addWork.disabled=true```
### CSS
- ```valami.classList.remove("ready")``` - add/remove/toggle
- ```valami.classList.replace("ready","not ready")``` - class csere
- ```valami.classList.contains("ready")```
- ```valami.style.backgroundColor="red"```

### Szám
- ```number.toFixed(2)``` - 2 tizedesre kerekít
- ```number.toExponential(2)```    // 123 ---> "1.23e+2"
- ```(123.456).toPrecision(2)    ```  // "1.2e+2"
- ```(123.456).toPrecision(4) ```     // "123.5"
- ```(123).toString(2)```             // "1111011"

### Date
- Aktuális idő: ```Date.now()```
- Megadott időpont
    - ```new Date(year, month, day [, hour, minute, second, millisecond]);```
    - ```Date.parse(dateString):``` ms
- Műveletcsoportok
    - getterek:``` getMonth(), getDay(),``` stb
    - setterek: ```setMonth(), setDay(),``` stb.
    - toString metódusok: pl. ```toLocaleString(), toGMTString()```
- Pl:
```
const d2 = new Date(2019, 0, 31, 3, 42, 0);
const d3 = new Date("2019-01-31T03:42:00");
d2.toLocaleString('hu-HU')  // "2019. 01. 31. 3:42:00"
d2.toLocaleString('en-US')  // "1/31/2019, 3:42:00 AM"

// Eltelt idő (ms-okban)
const delta = ms2 - ms1
```



# 3. rész - DELEGÁLÁS
## Eseményobjektum
- Általános tulajdonságok
    - pl. type, target
- Egéresemény
    - pl. screenX, screenY, buttons
- Billentyűzetesemény
    - pl. key, code, altKey, ctrlKey
### Gyakori függvények
- ```event.preventDefault()``` - pl linknél

## Buborékolás & Delegálás
- **Forrásobjektum:** az eseményt kiváltó objektum (e.target)
- **Események buborékolása:** az esemény a forrásobjektumtól kezdve sorban mindegyik szülőjén is bekövetkezik
<br>Lehetséges magasabb szinten kezelni az eseményt, mint ahol bekövetkezik
- **Kezelőobjektum:** az eseményre figyelő objektum
az az objektum, amelyhez az eseménykezelő hozzá van rendelve (this)
---
- **Delegálás:** az eseményt magasabb szinten kezeljük, de egy alacsonyabb szintű objektummal dolgozunk
- **Delegált objektum:** az az objektum, amellyel az eseménykezelőben dolgozni szeretnénk
### Esemény delegálása példa

```
const linkList = document.querySelector('#linkList');

function handleListClick(event) {
  if (!event.shiftKey) {
    return;
  }

  const target = event.target;

  if (!target.matches('li a')) {
    return;
  }

  event.preventDefault();
  const href = target.href;
  console.log(href);
}

linkList.addEventListener('click', handleListClick);
```
*Feladat: SHIFT gomb nyomva tartásával kattintva a konzolra kiírjuk a megnyitandó oldal URL-jét*

### "Rövid-okos" delegate segédfv
```
function delegate(parent, type, selector, handler) {
  parent.addEventListener(type, function (event) {
    const targetElement = event.target.closest(selector);

    if (this.contains(targetElement)) {
      handler.call(targetElement, event);
    }
  });
}
```
# Egyéb
## Táblázat generálás adott ```<table></table>``` tagbe
```
const map = document.querySelector("#map"); //táblázat neve
for (let i=0; i<rows; i++) {
  map.insertRow(i)
  for (j=0; j<cols; j++) {
    map.rows[i].insertCell(j)
  }
}
```


<footer style="text-align:center;">Made with ❤ by Liza</footer>
