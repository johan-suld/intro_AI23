# Arbetsflöde för AI-projekt – Johan Süld 

 *Om ni undrar varför det saknas referenser på vissa ställen så är anledningen att jag skapat linjära regressionsmodeller förut och minns vissa delar.*

---

### Beskriv hur man kan göra för att samla in datan, vilka format, vart kan man spara datan? 

Datat ska bestå av slutpris och ett antal variabler kopplat till sålda hus. Den kan inhämtas från t.ex. Hemnets API<sup>1</sup>. Datat kan sparas antingen i en databas eller som filer (txt, csv, xlsx etc.) på en hårddisk. För denna rapport väljer vi databas. För att lättast kunna använda datat senare bör den sparas som en enskild tabell, där raderna består av observationerna, dvs. varje sålt objekt och ett objekt får inte förekomma på mer än en rad. Kolumnerna ska bestå av slutpriset och variablerna, t.ex. antal rum och område. 

### Beskriv hur man kan visualisera datan? 

Datavisualiseringen kan göras t.ex. med Pythonbiblioteket Seaborn<sup>2</sup>. Lämpliga visualiseringar: 

- Fördelning av slutpris i form av ett histogram, graf eller boxplot. Detta för att få en uppfattning över prisklasserna modellen tränas på, är den tillräckligt täckande? 

- Korrelation mellan variabler som ett punktdiagram. Förklarande variabler i en regressionsmodell ska vara oberoende av varandra, dvs. variablerna får inte ha för hög korrelation mellan varandra<sup>3</sup>.

- Korrelation mellan slutpris och varje enskild variabel som ett punktdiagram för att få en uppfattning av vilka variabler som eventuellt har en signifikant effekt på slutpriset. 

- Antal förekomster av unika värden på kategoriska variabler, t.ex. med ett stapeldiagram. Finns det variabler där ett visst värde finns på nästan alla observationer? Om 99% av datasetet är hus som sålts i stockholmsområdet, kan det då ha blivit något fel i datainsamlandet? 

### Beskriv hur man kan göra för att bearbeta datan till rätt format? 

Målet är att transformera responsen från Hemnets API till tabellform innan den läses in i en databas. Detta kan göras t.ex. med Pythonbiblioteket Pandas. Med det kan man skapa kolumner, och verifiera att raderna ser bra ut t.ex. genom att ta bort eventuella dubbletter. Det är också bra att se till att värdena är i rätt format, heltal, decimaltal för numeriska variabler, och string för kategoriska. Även hantering av felaktiga eller tomma värden bör genomföras, ska man ta bort vissa observationer helt, eller ersätta felaktiga värden med 0, “n/a” eller kolumnens medelvärde? 

### Beskriv hur linjär regression fungerar 

Linjär regression tillåter oss att mäta sambandet mellan ett utfallsmått (slutpris i vårt fall) och en eller flera förklarande variabler. Använder man mer än en förklarande variabel kallas det multipel linjär regression. Analysen ger även svar på hur starkt sambandet är mellan slutpriset och varje enskild variabel, och med hjälp av detta kan vi skapa en prediktion av slutpris för kommande hus till salu<sup>4</sup>. Med datat vi samlat in får vi fram en modell eller ekvation där slutpris är en funktion av de variabler som visar sig ha en signifikant effekt på slutpriset. För varje variabel beräknas en koefficient eller vikt som är konstant för modellen. Om t.ex. koefficienten för storleken på huset i kvm är 10 000 så ökar slutpriset med 10 000 när storleken ökar med en kvm. Det går att använda sig av förklarande variabler som inte är numeriska om man använder sig av s.k. dummyvariabler<sup>5</sup>. Dessa delar upp kategoriska variabler, t.ex. område i flera nya variabler eller kolumner. Kolumnen område kan delas upp i kolumnerna område_a, område_b, ...område_n, där värdet blir 1 om huset tillhör området, och 0 annars. Utfallsmåttet för en linjär regression är alltid kontinuerligt<sup>4</sup>.

### Beskriv hur man kan göra för att driftsätta modellen? 

Man kan driftsätta modellen i en applikation som används för att predicera slutpris för hus som ännu inte sålts. Varje förklarande variabel kan ha ett eget inputfält och användaren fyller i dessa för det hus man är intresserad av, t.ex. antal rum, område etc. Appen tar inputen, lägger in dessa i modellen och beräknar det estimerade slutpriset som visas för användaren. Appen består i så fall av ett formulär som front end, och modellen och beräkningarna som back end. 

### Vilka teknologier kan man använda i de olika stegen i maskininlärningsprocessen 

För att samla in datat via API kan man använda ett Pythonbibliotek, t.ex. Requests<sup>6</sup>. Scriptet som hämtar datat, omvandlar det och sparar ned det kan byggas i PyCharm. SQL Server kan användas för att lagra det i en databas, och inläsningen kan ske via pythonscriptet mha. SQLAlchemy. Datat kan sedan hämtas upp i ett annat pythonscript via en SQL-query, och modellen kan tränas via biblioteket scikit-learn<sup>7</sup>.


### Referenser
1: [Hemnet BostadsAPI](https://integration.hemnet.se/documentation/v1#tag/Sale%2Fpaths%2F~1sales~1%7Bid%7D%2Fget)

2: [Top 8 Python Libraries for Data Visualization - GeeksforGeeks](https://www.geeksforgeeks.org/top-8-python-libraries-for-data-visualization/)

3: [Multicollinearity in Regression Analysis: Problems, Detection, and Solutions - Statistics By Jim](https://statisticsbyjim.com/regression/multicollinearity-in-regression-analysis/#:~:text=Multicollinearity%20occurs%20when%20independent%20variables%20in%20a%20regression,you%20fit%20the%20model%20and%20interpret%20the%20results.)

4: [Multipel regression (linjär regressionsanalys): teori, genomförande, tolkning, exempel - Mede](https://mede.se/lessons/multipel-regression-linjar-regressionsanalys-teori-genomforande-tolkning-exempel/)

5: [Regressionsanalys med dummyvariabler (stathelp.se)](https://www.stathelp.se/sv/dummy_sv.html)

6: [Top 15 Best Python REST API Frameworks (2022) | RapidAPI](https://rapidapi.com/blog/best-python-api-frameworks/)

7: [Multiple Linear Regression With scikit-learn - GeeksforGeeks](https://www.geeksforgeeks.org/multiple-linear-regression-with-scikit-learn/)