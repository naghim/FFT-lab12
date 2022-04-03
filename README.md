# Labor 12

## QML 

<p align="justify"> A QML egy deklaratív nyelv, amely lehetővé teszi a felhasználói felületek leírását vizuális összetevőik, valamint az egymással való kölcsönhatás és kapcsolat szempontjából. Ez egy jól olvasható, JSON-szerű szintaxist használó nyelv, amelyet arra terveztek, hogy lehetővé tegye a komponensek dinamikus összekapcsolását, könnyű újrafelhasználását és testreszabását. A QtQuick modul használatával a tervezők és fejlesztők könnyedén felépíthetik az animált felhasználói felületeket QML-ben, és összekapcsolhatják ezeket bármely C++ backend könyvtárakkal. A Qt Quick a QML típusainak és funkcionalitásának szabványkönyvtára. Ez magában foglalja a vizuális típusokat, az interaktív típusokat, az animációkat, a modelleket és a nézeteket, a részecske (<i>particle</i>) és az árnyékoló (<i>shader</i>) effektusokat. </p>

## Feladatok
1. Implementáljuk egy Quick (C++/QML – nem widgets) számológép alkalmazást. [Segítség](https://doc.qt.io/qt-5/qtdoc-demos-calqlatr-example.html)
2. Implementáljunk egy időjárás információ Quick alkalmazást. Az információ lekérésére használjuk az [OpenWeatherMap](https://openweathermap.org/api) API-t. [Segítség](https://doc.qt.io/qt-5/qtpositioning-weatherinfo-example.html)
 
<p align=center> <img src="https://i.ibb.co/SXwQvks/calqltr.png" align="center" width="200px"> <img src="https://i.ibb.co/vLwwDSZ/weatheringwithyou.png" align="center" width="180px"> </p>

3. _Aim Lab._ Készítsünk egy "játékot", melyben egy egérrel mozgatott célkereszt segítségével, különböző színű és méretű, a képernyőn véletlenszerűen mozgó körökre vadászunk és "lövünk". Egy körnek a mozgási sebességet és pontértékét a színe határozza meg. A köröknek az átmérője és áttetszősége (alpha kanális) idővel csökken, így nehezebb őket eltalálni és egy idő után teljesen eltűnnek. Mindig van egy tiltott (nem szabad meglőni) és egy kritikus (nem szabad elszalasztani) szín, ezek véletlenszerűen, időközönként változnak. Minden hibánál (tiltott megjövése, kritikus elszalasztása), veszítünk egy életet. A játéknak akkor van vége, ha elfogynak az életeink, amiből a játék indulásakor három van. További életeket gyűjthetünk, egy ritkán megjelenő és hamar elillanó, kicsi méretű, és rögzített irányban gyorsan mozgó kocka eltalálásával. A játékban opcionálisan hangeffektusokat (https://freesound.org) is lejátszhatunk a  [```QSound::play()```](https://doc.qt.io/qt-5/qsound.html) segítségével. (_Nem_ kell QML-lel megoldalni a feladatot) 

