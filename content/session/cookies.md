---
title: Cookies
order: 10
---
Bis jetzt war jeder Zugriff auf die Webapplikation unabhängig von jedem anderen
Zugriff: die PHP-Applikation weiß nicht, ob 10 verschiedene Leute die Homepage
abrufen oder ob eine Person die Seite 10 mal lädt.

![HTTP als "stateless protocol"](/images/stateless-http.svg)

Das ist eine Grundeigenschaft von HTTP: es ist „stateless“ (zustandslos).  Das
Gegenteil davon wäre „statefull“ (zustandsbehaftet).

Also kann man mit HTTP alleine – wie wir es bisher kennen – kein „Login“
schaffen. Um zu wissen, dass der User Bob eingeloggt ist müsste er ja
„wiedererkannt“ werden. Genau das macht den „state“ aus.

Cookies 
-------- 

Um das zu ermöglichen wurde das HTTP-Protokoll um die
sogenannten „Cookies“ erweitert: Ein Cookie besteht aus bis zu 4096 Bytes Daten,
die der Webbrowser lokal speichert, und bei jedem Zugriff auf den Webserver
wieder mitsendet. 

![Cookie wird gesetzt und bei jedem weiteren Request gesendet](/images/image363.png)

Der Browser sendet nie ein Cookie an einen anderen Webserver
als den von dem er es erhalten hat. Er kann aber viele verschiedene Cookies von
verschiedenen Servern speichern (In einem Cookie-Jar).


§

An Hand dieses Cookies kann eine Webapplikation einen bestimmten User
wiedererkennen. Cookies können als Teil des HTTP-Protocols (nur) vom Server
gesetzt werden. Dabei wird der Name des Cookies angegeben, der Wert der
gespeichert werden soll, und der Gültigkeitsbereich und Zeitraum:

<javascript>
Set-Cookie: style=gelb 
Set-Cookie: style=gelb; path=/admin/
Set-Cookie: style=gelb; expires=Tue, 29-Mar-2015 19:30:42 GMT; path=/admin/
</javascript>

Die einzige Art ein Cookie zu löschen ist ein Cookie mit gleichem Namen und
Ablaufdatum in der Vergangenheit zu setzen:


<javascript>
Set-Cookie: style=wurscht; expires=Tue, 29-Mar-2005 19:30:42 GMT; path=/admin/
</javascript>

### Cookies in PHP


In PHP finden Sie die bereits gesetzten Cookies, die vom Browser zurückgesendet
wurden im Array `$_COOKIES`[*](http://php.net/manual/en/reserved.variables.cookies.php) 
(und — zusammen mit GET und POST-Parametern — in $_REQUEST). 

Neue Cookies können Sie mit `setcookie()`[*](http://www.php.net/manual/en/function.setcookie.php) setzen. 


### Cookies in JavaScript

Im Client können Cookies mit JavaScript gelesen und geschrieben werden. Dies
funktioniert über eine Zuweisung an die Eigenschaft document.cookie.

Achtung: beim lesenden Zugriff auf diese Eigenschaft enthält man einen String
mit allen Cookies die gerade gültig sind. 

Zum Setzen von neuen Cookies wird auf die Eigenschaft zugewiesen. Zum Setzen
mehrere Cookies wird wiederholt zugewiesen — das hat aber (noch) keinen Einfluß
auf den Wert den man aus document.cookie auslesen kann:

<javascript>
alert(document.cookie);  // Zeigt die gültigen Cookies

document.cookie = "farbe:grün"; document.cookie = "anzahl: 3"; 
document.cookie = "passwort: total super geheimes aber recht langes passwort";
alert(document.cookie);  // Zeigt immer noch die ALTEN Cookies !!!
</javascript>

Erst wenn ein neuer Request an den Server erfolgt, werden die neu gesetzten
Cookies mit gesendet: erst wenn die Antwort wieder dargestellt wird, sind die
neuen Cookies in JavaScript über document.cookie lesbar!

### Libraries für Cookies in JavaScript

Um das Lesen, Setzen und Löschen von Cookies in JavaScript zu vereinfachen git
es zwei gute Libraries:

* die Funktionen von [Quirksmode](http://www.quirksmode.org/js/cookies.html) ein
* jQuery Plugin [jquery-cookies](https://github.com/carhartl/jquery-cookie)

