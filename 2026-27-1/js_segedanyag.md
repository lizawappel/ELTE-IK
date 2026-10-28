# 1. rész - TÖMBFÜGGVÉNYEK
## Alapok
- forEach: általános ciklus
- some: eldöntés
- every: optimista eldöntés
- map: másolás
- filter: kiválogatás
    ```
    const evens = numbers.filter(e =>e % 2 === 0);
- reduce: összegzés (sorozatszámítás)
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
- find: keresés (elem)
- findIndex: keresés (index)





## Példák a mintaZH-ból
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
- const elemById = document.querySelector("#id")
- const elemsByClass = document.querySelectorAll(".class")
```
const pages = document.querySelectorAll('.page');
pages.forEach(p=> p.style=`width: ${pace.value*20}px`)
```

## Value, innerHTML, textContent érdekesek
- edition.selectedOptions[0].value --- optionsnál az adott option értéke

## Gyakori függvények
### String
- ```szoveg.trim()``` - leszedi a whitespacet 
- ```szoveg.toLowerCase()```
### Tömb
- ```tomb.push(element)```
- ```tomb.includes(element)```
### HTML
- ```const valami = createElement("li")```
    - ez pl csinál egy listitemet
- ```parentElement.appendChild(childElement)```
    ``` 
    const listItem = document.createElement("li");
    listItem.innerHTML=input;
    readingList.appendChild(listItem);
    ```
- ```addWork.disabled=true```
### CSS
- ```planner.classList.remove("ready")``` - add/remove/toggle
- ```valami.style.backgroundColor="red"```

### Egyéb
- ```number.toFixed(2)``` - 2 tizedesre kerekít





<footer style="text-align:center;">Made with 💔 by Liza</footer>
