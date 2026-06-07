### Allmänt

UTF-8 ska gälla.

Skapa följande filer:

* sketch.js
* index.html

Programmet ska hantera ett antal bergergrupper med n deltagare i varje grupp.  
Med n deltagare blir det n-1 ronder.  

Antag att vi har 48 spelare.

Dessa ska lottas i 12 bergergrupper med tre ronder

Gruppnamnen är A B C D E F G H I J K och L.

I navigeringsinformationen vill jag att • används istf kommatecken som separator

Hjälptexten "Mata in varje resultat två gånger" behövs ej.

Turneringsnamnet behöver bara anges en gång.  
Dock ska det användas även i fliknamnet.  

Indikera grupperna med en två pixels horisontell linje.

Denna tjockare linje bör även användas av tabellernas ytterkanter samt linjen under th.

Markera aktuell rad med en diskret blå vänsterkant i resultatcellen.

### Url till en turnering med anmälda

Jag behöver en webapplikation som accepterar en parameter n för gruppstorlek samt en lista med spelarnamn.

Exempel:

http://127.0.0.1:5500/#id=16442&turnering=Växjöspelen 2026&n=4&players=1984 Adam Nilsson_1954 Bertil Svensson_1812 Cesar Persson_1776 David Eriksson_1912 Erik Karlsson_1917 Filip Jönsson_2026 Gustav Hansson_1945 Helge Ågren

* De första fyra tecknena utgör elo-talet.
* Lottningen behöver ej stå i urlen, den är samma varje gång.

Resultaten lagras i urlen i bordsordning:

r1=110r010011rr0011rr00rr11  
r2=110r010011rr0011rr00rr11  
r3=110r010011rr0011rr00rr11  

Remi visas alltså med r.

Urlen uppdateras för varje inmatat resultat

Markera i urlen att inga parametrar behöver skickas till servern med #. Detta för att minska roundtrippen.  
Parametrarna ska stå i ordning: id, turnering, n, players, r1, r2 ...

Använd history.replaceState för att minska mängden länkar i historiken

Bergerlottningen ska skötas med hjälp av berger_4.js

### Bordslistan ser ut så här, 24 rader

Lämplig rubrik

```
Grupp Bord Vit   Resultat Svart
A     1    Adam     -     Bertil
A     2    Cesar    -     David
B     3    Erik     -     Filip
osv
```

### Standings ser ut så här, 48 rader

Lämplig rubrik

```
Grupp Id Namn Elo  1   2   3   Poäng
A     1  Adam 1234 2w= 3b1 4w0 1.5
osv
```

Centrera rondnummer.

### Navigering med tangenter

Left, Right: Byter rond  
Up, Down: Byter bord  
Ovanstående kommandon ska visas som pilar  

`1` = vit vinst  
`0` = vit förlust  
space = remi  
`r` = remi  
`+` = vit vinst w.o.  
`-` = vit förlust w.o.  

space/r ska visas på en enda knapp  
w.o. ska visas bara med + och -  

Backspace = ställer sig på förra raden och tar bort resultat

Tangenterna `1`, `0`, space, `r`, `+`, `-` innebär alltid att "cursorn" flyttas till nästa bord.

w.o. ska visas med `+ - -` och `- - +`.

Grupp ska förkortas till G

Dessa kommandon ska även vara knappar.

Hjälpraden kan tas bort.

Visa hur många resultat som matats in samt antal partier totalt. 

Placera "8 av 12 resultat" till höger om de fyra piltangenterna.

Gör turneringsnamnet till en länk till originalturneringen på medlemssystemet.  
Använd parametern id som har värdet 17900 i exemplet nedan.  
https://member.schack.se/ShowTournamentServlet?id=17900  

### Kontroll av inmatning

Man ska kunna mata in resultaten två gånger för att säkerställa korrekt inmatning.
Då man trycker på 10r andra gången flaggas mismatch med röd färg.
Backspace måste användas om man vill ändra ett resultat.

Exempel:  
Första inmatning: 1r0  
Andra  inmatning: 100  

Nu ska det andra bordet visa rött. Då går man dit, trycker på Backspace och matar in korrekt resultat

### Sortering av Standings

1. Grupp
2. Poäng
3. Inbördes möte (om möjligt)
4. Sonneborn-Berger
5. Antal vinster