Bevezetés a BrainFuck Programozásba v.1.4
Utolsó módosítás: 2006.06.10.

--[1] BEVEZETŐ

A BrainFuck valószínűleg a legőrültebb nyelv, amelyhez valaha szerencsém volt. 
És igen, meglehetősen kevés leírás van, amit a google-el találhatsz ezen 
nyelvről és programozásról. De írtam nektek egyet, remélhetőleg némi betekintést 
nyújtva a nyelvbe. A legtöbb, ami elérhető, az is csak az operátorok 
alaphasználatát mutatja be. A lényeg az, hogy a nyelv magában egy "Turing-képes"
nyelv, amelyet Urban Müller készített. (célja egyébként a világ legkisebb 
fordítójának elkészítése volt - ford.) A nyelv csupán 8 operátort tartalmaz
 +-[],.<> ezekkel képest lehetsz bármilyen programozási probléma megoldására.


--[2] ALAPOK

A BrainFuck alapgondolata a memória manipuláció. Alapvetően neked van egy 30,000 
elemszámú tömböd, 1 byetos memória blokkokkal. A tömb mérete a complieter 
(fordító) vagy az interpreter implementációin múlik, de a BraiFuck-ban ez 
általában 30,000. Mozgásra a blokkok között egyetlen pointerünk ad lehetőséget. 
Ezután már csak az itt található értékek növelésére és csökkentésére van módunk. 
Had mutassam meg, mire is jó a mi kis nyolc operátorunk:

> = növeli a memória pointert, vagyis átugrasztja egy blokkal jobbra
< = csökkenti a memória pointert, vagyis  átugrasztja egy blokkal jobbra 
+ = növeli a blokk értékét, amin a pointer áll
- = csökkenti a blokk értékét, amin a pointer áll
[ = mint a C nyelvben a while(akutalis_blokk_erteke != 0) ciklus
] = ha a blokk aktuális eleme amire a pointer mutat nem nulla, akkor visszaugrik 
a [ jelhez
, = mint a C-ben a getchar(), bekér egy karaktert
. = mint a C-ben a putchar(), kiir egy karaktert

Pár szabály:

- Bármilyen más, tetszőleges karakter a nyolcon kívül el lesz utasítva a fordító 
által. A karakterek a 8 speciálison kívül kommenteknek számítanak.

- Minden memória blokk a "tömbön" belül 0 értéket vesz fel a program futatásának 
kezdetén. A pointer mutató bal oldalról indul.

- Ciklusokból annyi használható amennyi csak jól esik, de minden [ -hez kell 
tartoznia egy ] -nek is.

Kezdjük pár alapvető példával, hogyan kell BrainFuck-ban programozni.

A legegyszerűbb program BrainFuck-ban a következő:

	[-]

A program a következőket teszi. Belép a ciklusba, majd csökkenti az aktuális 
értéket 1-gyel, amíg az a 0-t el nem éri, ekkor kilép. De mivel minden blokk 0 
kezdőértéket tartalmaz, ebben az esetben egyszer sem lép be a ciklusba. Na 
írjunk valami tényleg működő programot.

	+++++[-]
   
Ez C-ben így néz ki:

	*p=+5;
	while(*p=!0){
		*p-;
	}

A programunkban az aktuális memória blokkban 5-re növeljük az értéket, majd 
belépünk a ciklusba, ami 0-ig csökkenti az értéket, majd kilép.

	>>>>++

Ezzel a memória pointert a negyedik blokk-ra állítjuk, majd ott az értéket 2-re 
növeljük. 

Így nézhet ki:

	memória blokkok
	--------------
	[0][0][0][2][0][0]...
		  ^
		memória pointer

Ahogy látszik a ASCII rajzon, a mi memória pointerünk a negyedik blokk-ra mutat, 
és megnövelte az értéket. Ha előtte semmi nem történt, akkor most a 2 értéket 
vette fel. Ha egy kicsit még alakítunk a programot és írunk valamit hozzá:

	>>>>++<<+>>+
	
A program végeztével a memória valahogy így néz ki:

	memória blokkok
	-------------
	[0][1][0][3][0][0]...
		  ^
		memória pointer

A pointer a negyedik blokkig elmegy, kettővel növeli az értéket, majd kettő 
blokk-kal visszamegy, és a második blokk-ban egyel növeli az értéket. Ezek után 
a pointert kettővel jobbra megy, és újra a negyedik blokkban, növeli az értéket, 
egyel. Ez eddig szép, és jó, de igazából eddig semmit sem láthattunk ebből. 
Csináljuk valamit, ami immár vissza is jelez valahogyan.

Az ősember, amikor elkezdett rajzolni a falra (az eredeti szövegben ott volt, 
hogy mivel, ha érdekel, járjatok utána :) - ford.), az első kép, amit rajzolt 
egy ember volt, aki integet a földön. 
Nem akarom megtörni a hagyományt, így lássunk egy Hello Word!-öt a BrainBuck-
ban:


	>+++++++++[<++++++++>-]<.>+++++++[<++++>-]<+.+++++++..+++.[-]
	>++++++++[<++++>-] <.>+++++++++++[<++++++++>-]<-.--------.+++
	.------.--------.[-]>++++++++[<++++>-]<+.[-]++++++++++.

Emlékezzünk, hogy mi számokkal dolgozunk, így tehát muszáj a karakterek ASCII 
kódját használni a megjelenítéshez. Daraboljuk fel a programunkat.

	>+++++++++[<++++++++>-]<
	
Kezdjük az elején, használjuk közben végig, a már jól ismert memória ábránkat.

	>

Az első, amit látunk, hogy a memória pointerünket a következő blokkra 
ugrasztjuk, az első blokkot pedig nulla értéken hagyjuk.

	memória blokkok
	-------------
	[0][0][0][0][0][0]...
	    ^
	memória pointer

Az aktuális blokkban 9-re növeljük az értéket.

	+++++++++
	
A ábrát nézve ezt látjuk:

	memória blokkok
	-------------
	[0][9][0][0][0][0]...
	    ^
	memória pointer
	
Mivel a blokkban nem 0 van, így belépünk a ciklusba.

	[

Most a ciklusban vagyunk. Ekkor a pointert eggyel balra mozgatjuk.

	<

Ekkor ez a helyzet áll fent:

	memória blokkok
	-------------
	[0][9][0][0][0][0]...
	 ^
	memória pointer

És növeljük az értékét 8-ra.

	++++++++

Az ábránk tehát így néz ki most:

	memória blokkok
	-------------
	[8][9][0][0][0][0]...
	 ^
	memória pointer

Majd jobbra lépünk egy blokk-kal (a második blokkba újra vissza) és egyel 
csökkentjük az ott tárolt értéket, 9-ről 8-ra.

	>-

Ábra:

	memória blokkok
	-------------
	[8][8][0][0][0][0]...
	    ^
	memória pointer

Vége a ciklus bezárójához érünk:

	]

Ami ellenőrzi, hogy az aktuális elem (amin a pointer áll) nem-e nulla. Mivel 
jelen esetben nem nulla, hanem 8, a ciklus előrül kezdődik. A második lefutás 
után így néz ki a memória blokk szelet:


	memória blokkok
	-------------
	[16][7][0][0][0][0]...
	     ^
	memória pointer

Ez addig folytatódik, amíg a második blokkban lévő érték el nem éri a 0-t. Akkor 
kilép a ciklusból. Ha egyszer kilépett, akkor az első blokkra mozgatja a 
pointert, és megjeleníti az ott lévő értéket. Ha sikerült követni a folyamatot, 
akkor észrevetted, hogy 9 alkalommal növeltünk 8-cal az első blokk 
értékét. Mivel tudjuk, hogy 8*9 = 72 és a 72 ASCII értéke decimálisan 'H' már 
meg is kapuk az első betűt

	<

	memória blokkok
	-------------	    
	[72][0][0][0][0][0]...	    
	  ^    
	memória pointer	    
	
Hívjuk a megjelenítő (aka putchar()) operátort, és egy 'H' betűt rajzol ki a 
képernyőre.

	.

Wow, ennyi munka és csak egyetlen karaktert jelenítettünk meg.  Kérdezheted, 
hogy miért vesztegessem az időm, hogy ilyen borzasztó használhatatlan 
programozási nyelvvel szenvedek?! Hát azért, mert páran a mi hackereink közül 
szeretik a mókás dolgokat, a kihívásokat, és tornáztatni az elméjüket. (Valamint 
tök jó a nyelv zero-memory alkalmazások írására - ford.)

Ha C-ben irtuk volna meg így nézne ki:

	++p;
	*p=+9;
	while(*p != 0){
		--p;
		*p=+8;
		--p;
		--*p;
	}
	--p;
	putchar(*p);

	
A Hello World! további részének kibogozását már rátok bízom, most már van 
valamennyi alapotok a memória manipuláció terén.


--[3] BEMENET

Az input (bekérés) a BrainFuck-ban a ',' operátor végzi. A bevitt karakter ASCII 
kódját kapjuk meg, és tároljuk el az aktuális memória blokkban. Tehát ha te a 
billentyűzeten leütöd a 2-est, nem 2-őt, mint értéket tároljuk el, hanem a 2 
ASCII kódját ami esetünkben az 50.

	,.,.,.

Ez a kis program bekér három karaktert, és azonnal ki is írja őket. Csináljuk 
valamivel komplexebben.

	>,[>,]<[<]>[.>]

Ez a program hasonló az UNIX-os cat parancshoz. Az STDIN-ről olvassa az értéket, 
és az STDOUT-on jeleníti meg. (STDIN - standart iput, STDOUT - standart output. 
- ford.) Daraboljuk fel:

	>,

Memória pointert jobbra tolása a második blokkig. Az input a második blokkba 
kerül, az első üresen marad.

	[>,]

Ciklus kezdődik, ami a következő blokkra ugraszt, és eltárolja oda a bevitt 
értéket. Ez addig fog folyamatosan menni, amíg NULL karakter nem érkezik (\0 
vagy a 0 ASCII értéke).

	<[<]

Visszatekerés :) Ha ide elértünk a programunkkal, az azt jelenti, hogy előbb 
NULL karakter volt a bemeneten, tehát a memória pointert most egy olyan blokkon 
áll, amiben 0 van. Ekkor viszont nem lépnénk bele a ciklusba. Ezért, eggyel 
balra lépünk (oda ahol már nem nulla van) és akkor kezdődik a ciklus, ami 
visszaléptet egészen addig, amíg ismét nulla-t nem talál. Ez pedig pontosan az 
első memória blokkot fogja jelenteni, hiszen úgy kezdtük, hogy az első érték 
tárolás a második blokkba került. (>,) Ha eléri az elejét, kilép a ciklusból.

	>[.>]

Most a memória blokkok legelején vagyunk. Eggyel jobbra lépünk (0-t kikerüljük), 
majd ismét egy ciklus jön, ami abból áll, hogy az aktuális pointer alatt lépő 
értéket megjelenítjük, majd átlépünk a következőre. Ez szintén addig fog 
haladni, amíg el nem érünk oda, ahol NULL karakterbe ütötközünk.

C-ben ez így néz ki:

	++p;
	*p=getchar();
	while(*p != 0){
		++p;
		*p=getchar();
	} 
	--p;
	while(*p != 0) --p;
	++p;
	while(*p !=0){
	putchar(*p);
	++p;
	}

Most már képesek vagyunk kezelni az input/output-ot, és a memória manipulációval 
is elboldogulunk.


--[4] TRÜKKÖK

Nagyon sok kis trükk van, amelyek segítségével könnyebbé tehetjük a BrainFuck 
használatát. Megpróbálok párat bemutatni.

Hogyan helyezünk át, vagy shifteljünk értéket egyik memória blokkból a másikba:

	+++++[>>+<<-]

Ez beállítja az első blokk értékét 5-re. Jön egy ciklus, ami kettővel jobbra 
lépteti a pointert, majd ott éréket növel. Visszalépünk kettőt (az első memória 
blokk-ra) és ott csökkentjük az értéket eggyel. Ez egészen addig megy, amíg az 
első blokkban nulla nem lesz az érték.

Hogyan másoljuk egyik blokkból a másikba:

	+++++[>>+>+<<<-]>>>[<<<+>>>-]

Ez a kis program 5-re állítja az első blokkot. Aztán a harmadik és a negyedik 
blokkba másolja őket, úgy, hogy az első blokkot kiüríti. Ezek után a negyedik 
blokkból áthelyezi az értéket az elsőbe vissza, végül a negyedik üres marad.

Két blokk összeadása nem is olyan bonyolult:

	+++++>+++[<+>-]

Első blokk értékét 5-re. Pointert átugrasztjuk a másodikra, ott a 3 értéket 
állítjuk be.  Most az második blokkban lévő értéket akarjuk az elsőhöz 
hozzáadni, tehát belépünk a ciklusba, ami visszalépteti eggyel a
pointert, ott értéket növel, ezután előre viszi eggyel a pointert (a kettes 
blokkra) és ott értéket csökkent. Ez ugye addig megy, amik a második blokk 
értéke el nem éri a nullát.

A kivonás sem sokkal bonyolultabb:

	+++++++>+++++[<->-]

Első blokk értéke 7-re. Eggyel jobbra visszünk a pointert, és ott 5-re növeljük 
a blokk értékét. Majd jön a ciklus, ami eggyel visszalép (az első blokkra) ott 
csökkenti az értéket, majd visszalép a második blokkra, és ott is csökkenti az 
értéket. A ciklusból akkor lép ki, amikor a második blokkbnulla lesz az érték, 
ami ugye azt eredményezi, hogy a 7-et ötször csökkentettük, így megvalósult a 
kivonás.

Szorzás, már egyszer szerepelt a leírásban, a hello word kapcsán, de itt most 
még egyszer láthatjátok:

	+++[>+++++<-]

Tehát, az első blokk értékét 3-ra állítjuk, majd jön a ciklus, ami eggyel jobbra 
lépteti a pointert, és ott 5-re növeli a blokk értékét. Majd visszalép az első 
blokkra, és csökkenti az ott lévő értéket. Látható, hogy a ciklus addig fog 
végrehajtódni, amíg az első blokk értéke el nem éri a nullát. Eddig összesen 
háromszor fog lefutni a ciklus, ami azt jelenti, hogy háromszor 5-tel növeli a 
második blokk értékét.

Az osztás hasonlóképpen van, csak kivonás van az összeadás helyett.

--[5] FELTÉTELES SZERKEZET

Első ránézésre talán a feltételes szerkezetek megvalósítása tűnik a 
legnehezebbnek a BrainFuck nyelvben. Ha belegondolsz, egy feltételes szerkezet 
nem más mint, hogy megvizsgálod egy állításról, hogy az IGAZ vagy HAMIS. Vagy 
egyszerűbben, 0 vagy nem 0. Tehát megpróbálom bemutatni a nyelv legkevésbé 
dokumentált részét, a feltételes szerkezeteket.

Tegyük fel, hogy a memória blokkban van egy egyes értékű bemenetünk. Ezután azt 
szeretnénk, hogy HA a bemenet (x) egyenlő 5-tel (y) értékét 3-ra állítjuk. Ezt 
kétféleképpen tehetjük meg. Van a "destructive" mód, melynél a vizsgált értéket 
csökkentjük, valamint a "non-destructive", ahol a változó érintetlen marad.

Példa a "non-destructive" módra:

C nyelven:

     x=getchar;
     if(x == 5){
       y = 3;
     }  


BrainFuck nyelven:

     ,[>>+>+<<<-]>>>[<<<+>>>-]>+<<[-----[>]>>[<<<+++>>>[-]]


A biztos megértés kedvéért nézzük át részletesen. Vizsgáljuk meg kétszer. 
Először a bemeneti érték 6, másodjára 5. Emlékezz, egy BrainFuck ciklus _csak_ 
akkor indul el, ha a memória blokk értéke, amire a pointer mutat nem nulla! Ha 
az aktuális blokk értéke nulla, a futás során a ciklus átugródik.

Bemeneti érték olvasása x-be.

     ,


      1  2  3  4  5  6
     [x][y][0][0][0][0]...
      ^
memória pointer


az értéket az 1-es blokkból a 3-asba másoljuk, a 4-es blokkot ideiglenes 
tárolóként használva. A végén a pointer a 4-es blokkon áll meg.

     [>>+>+<<<-]>>>[<<<+>>>-]


      1  2  3  4  5  6
     [x][y][x][0][0][0]
               ^
         memória pointer


az 5-ös blokk értékét egyre állítjuk. Ezen a blokk segítségével fogjuk tesztelni 
az állításról, hogy igaz vagy hamis. 

Ezután a pointert visszaállítjuk a 3-as blokkra.

      >+<<


      1  2  3  4  5  6
     [x][y][x][0][1][0]
            ^
      memória pointer


Most kivonunk 5-öt és ha x értéke öt, y értékét 3-ra állítjuk, majd a pointert 
az 5-ös blokkhoz mozgatva annak értékét nullára állítjuk így a ciklus csak 
egyszer fut le.

Ha x értéke nem 5 volt a pointer a 6-os blokkon áll meg.

     [-----[>]>>[<<<+++>>>[-]]


ha x 5:

      1  2  3  4  5  6
     [x][3][0][0][0][0]...
                  ^
            memória pointer

ha x nem 5:

      1  2  3  4  5  6
     [x][y][x][0][1][0]...
                     ^
               memória pointer

Ez volt a "non destructive" módja a feltételes kifejezéseknek. A "destructive" 
módnál a másolgatás helyett saját helyén, közvetlenül vonjuk ki az input 
értékből a megfelelő számot. Ez sokkal rövidebb kódot eredményez. 

"destructive" mód:

     >>+[<<,-----[>]>>[<<+++>>[-]]]


--[6] GYAKORLÓ FELADATOK:

1. Írj programot, ami kiírja a neved. (Példák: bsh.bf, scottanyo.bf)
2. Írj programot, ami kiírja az összes "nyomtatható" ASSCI karaktert. (Példa: 
assci.bf)
3. Írj egy programot melynek bemenete egy '\0' karakterrel lezárt sztring,
kimenete pedig egy BrainFuck kód, amit lefuttatva kiírja a begépelt sztringet.
(igen, elsőre kicsit vadul hangzik, de nem lehetetlen). 

--[7] LINKEK:

http://esoteric.sange.fi/brainfuck/ - minden mi szem-szájnak ingere
http://www.muppetlabs.com/~breadbox/software/tiny/bf.asm.txt - fordító
http://www.iwriteiam.nl/Ha_bf_intro.html - másik leírás (angol)
http://www.iwriteiam.nl/Ha_bf_numb.html - nagy számok kezelése a BrainFuck-ban


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

BrainFuck Programmin Tutorial
by: omin0us <omin0us208[AT]gmail[DOT]com>    

Fordította: scottanyo <scottanyo[AT]gmail[DOT]com> és B$H <bsh[@]gyorikredit[.]hu>
Köszönet: Eltono, zYztem
Fordítói megjegyzések:
	Nem szó szerinti fordítás, ahol úgy gondoltam, hogy kell még magyarázni,
	oda beletettem, ahol meg túl soknak gondoltam a rizsát, onnan elvettem.
	<scottanyo>
	Hozátenném, hogy fordításnak indult a dolog, de a végére egy sokkal 
	teljesebb mű kerűlt ki a kezünk alól. Több helyütt saját gondolatok
	is be lettek csempészve a fordítói megjegyzéseken túl, valamint
	kiegészítettük pár fejezettel a leírást. Egy helyütt a BrainFuck, szintén
	egy helyütt a C kód hibás volt, azt javítottuk. Hamarosan teszünk fel a
	feladatokhoz saját megoldásokat, valamint saját fordítót és toolokat.
	<B$H>

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
EOF

