# Labor 12

## Networking

<p align="justify"> A Qt-ban lehetőség adódik az internetről letölteni különféle adatokat. Akár JSON objektumokat, akár nagy méretű állományokat szeretnénk letölteni, használhatjuk a Qt-ban a <a href="https://doc.qt.io/qt-6/qtnetwork-index.html"><code>network</code> modult</a>. A network modul támogatja a sima socket alapú kommunikációkat, illetve a HTTP(s) alapú kapcsolatokat is.</p>

<p align="justify">A network modul használatához szükséges beilleszteni a projekt állományba, amely qmake esetén a következő módon történik:</p>

```qmake
QT += network
```

<p align="justify">CMake használata esetén:</p>

```cmakelists
find_package(Qt6 REQUIRED COMPONENTS Network)
target_link_libraries(mytarget PRIVATE Qt6::Network)
```

<p align="justify">A network modull aszinkron módon végzi el a hálózati hívásokat, így nem veszi el a felhasználói felülettől a futtatási jogot (nem blokkolja a Qt main threadet). Ahhoz, hogy választ kapjunk a network modulltól, használhatjuk a <a href="https://doc.qt.io/qt-6/qnetworkreply.html#signals"><code>QNetworkReply</code> által szolgáltatott eseményeket</a>. Ha nagyobb állományt töltünk le, használhatjuk a <a href="https://doc.qt.io/qt-6/qnetworkreply.html#downloadProgress"><code>downloadProgress</code></a> nevű eseményt, hogy valós időben visszajelzést kapjunk a letöltés állapotáról. A <a href="https://doc.qt.io/qt-6/qnetworkreply.html#finished"><code>finished</code></a> esemény segítségével le tudunk futtatni saját kódot, amennyiben véget ért a letöltés.</p>

Példa JSON adatok letöltésére és feldolgozására, publikus API-t felhasználva:

```cpp
// Lekérjük a hőmérsékletet egy publikus API-ról
QNetworkAccessManager manager;
QNetworkRequest request;
QUrl url = QUrl("https://wttr.in/Targu+Mures?format=j1");
request.setUrl(url);
QNetworkReply *reply = manager.get(request);

// A network modul aszinkron módon kéri le az adatokat.
// Ezért szükséges rácsatlakoznunk egy callback függvénnyel
// Qt-ben ezt az eventekkel valósítjuk meg
QObject::connect(reply, &QNetworkReply::finished, [&]() {
    // Ha nem történt hiba a kérés során...
    if (reply->error() == QNetworkReply::NoError) {
        // Beolvassuk a HTTP kérésből a választ
        QByteArray responseData = reply->readAll();

        // A választ alakítsuk át JSON objektummá, hogy könnyebben tudjuk feldolgozni
        QJsonDocument jsonResponse = QJsonDocument::fromJson(responseData);
        QJsonObject jsonObject = jsonResponse.object();

        // Ez a lista tartalmazza az eddig mért időjárásokat
        QJsonArray currentCondition = jsonObject.value("current_condition").toArray();

        // A legelső érték a JSON listában a legújabb időjárás
        QJsonObject firstCondition = currentCondition.first().toObject();

        int tempC = firstCondition.value("temp_C").toString().toInt();
        QJsonObject weatherDesc = firstCondition.value("weatherDesc").toArray().first().toObject();
        QString weatherDescription = weatherDesc.value("value").toString();

        qDebug() << "Hőmérséklet Celsiusban: " << tempC;
        qDebug() << "Időjárás leírása:" << weatherDescription;
    } else {
        // Ha történt hiba, írjuk ki a felhasználónak
        qDebug() << "Error:" << reply->errorString();
    }

    reply->deleteLater();
});
```

<p align="justify">A linkre kattintva megtekinthető a böngészőben is, hogy hogyan néz ki ez a JSON válasz: <a href="https://wttr.in/Targu+Mures?format=j1">https://wttr.in/Targu+Mures?format=j1</a>.</p>

## Animációk Qt-ben

<p align="justify">Qt-ben nagyon egyszerű az objektumok animálása is. Az animációkra használhatóak a <a href="https://doc.qt.io/qt-6/qpropertyanimation.html"><code>QPropertyAnimation</code></a>, amelynek meg lehet adni, hogy honnan kezdődjön az animáció, hol végződjön az animáció, milyen sebességgel történjen, és milyen görbét írjon le az animáció (például <a href="https://doc.qt.io/qt-6/qeasingcurve.html"><code>QEasingCurve</code></a>). A <a href="https://doc.qt.io/qt-6/qpropertyanimation.html"><code>QPropertyAnimation</code></a> segítségével az objektumunk egy bizonyos tulajdonságát módosítjuk az idő függvényében, mint például a pozícióját vagy az elforgatási szögét.</p>

## Feladatok

1. Készítsünk egy kezdetleges időjárást szemléltető applikációt! Használjuk a fent említett <a href="https://wttr.in/Targu+Mures?format=j1">wttr.in</a> publikus API-t. Jelenítsük meg a mért hőmérsékletet Celsius fokban, a mérés idejét, a légnedvességet (humidity) és az érzékelt hőmérsékletet Celsius fokban (feels like C). A felhasználói felületen jelenítsük meg az időjáráshoz megfelelő ikont és leírást is, illetve animáljuk meg az ikont (például napos idő esetén a nap forogjon körbe-körbe).

2. Készítsünk egy alkalmazást, amely segítségével letölthetünk HTTP(s) segítségével egy nagyobb méretű állományt. Használhatjuk a következő linket egy 50 MB méretű állomány letöltésére: <a href="https://sabnzbd.org/tests/internetspeed/50MB.bin">https://sabnzbd.org/tests/internetspeed/50MB.bin</a>. A letöltendő linket adjuk meg egy <a href="https://doc.qt.io/qt-6/qlineedit.html"><code>QLineEdit</code></a> segítségével. A letöltés státuszát jelenítsük meg egy <a  href="https://doc.qt.io/qt-6/qprogressbar.html"><code>QProgressBar</code></a> segítségével a felhasználónak! A sikeres letöltés esetén jelenítsünk meg egy <a href="https://doc.qt.io/qt-6/qmessagebox.html"><code>QMessageBox</code></a>-ot. Sikertelen letöltés esetén ugyancsak egy <a href="https://doc.qt.io/qt-6/qmessagebox.html"><code>QMessageBox</code></a> segítségével informáljuk a felhasználót.
