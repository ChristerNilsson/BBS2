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

http://127.0.0.1:5500/#id=16442&turnering=V%C3%A4xj%C3%B6spelen+2025&n=4&players=2188+Julius+Schwartz_2123+CM+Max+Wahlund_2233+Carlos+T%C3%B6rnberg_2118+CM+Anders+Wengholm_2107+CM+Nedzad+Neretljak_2029+Martin+G%C3%B6ransson_2107+FM+Conny+Holst_2105+Sebastian+Persson_1998+Kristian+Eriksson_2020+Lennart+Fransson_1999+Simon+Gustafsson_2000+Nejib+Bouaziz_1984+Christoffer+Stenman_1995+H%C3%A5kan+Winfridsson_1966+Isac+Johansson_1993+Lukas+Guagliano_1942+Lukas+Sp%C3%A5ng_1936+Roy+Berg_1964+Andreas+Peitersen_1960+Joakim+Eriksson_1936+Marcus+Johnsson_1896+Eva+Johansson_1892+Anders+Kvarby_1881+Ofelia+Th%C3%B6rnqvist_1833+Karl+Malbert_1852+Arnold+Hermansson_1841+Sten+Bernhardsson_1827+Fredrik+Qwarfort_1824+M%C3%A5rten+Garner_1792+Nils+Zandler_1814+Minh+Thuc+Le+Doan_1824+Bj%C3%B6rn+Ottosson_1775+Lukas+Rasmussen_1725+Joakim+Wahlstr%C3%B6m_1728+Bengt+Svensson_1735+Per+D%C3%A4ldborg_1717+Martin+Sedl%C3%A1k_1685+Paul+Eknor_1723+Sven-Ingvar+Sundin_1713+Liam+Blixth_1657+Julius+M%C3%A4lming_1614+Duy+Thuc+Le+Doan_1683+Liron+Liebe_1670+Marvin+Mulato_1587+Michael+Persson_1614+Leif+Westman_1573+Anders+Hansen_1575+Nellie+St%C3%A5hl_1525+Andreas+Lundstr%C3%B6m_1529+Stefan+Magnusson_1509+Theodor+Cornfors_1540+Olof+Johannesson

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