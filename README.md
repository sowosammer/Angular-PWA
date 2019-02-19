# Angular-PWA
Notizen etc. zu PWA

Fragen: 
Synchronisierung Cachng Browser <-> PWA
Observable oder Promise -> obs.ready um zu warten bis antwort kommt.

Download: http://tinyurl.com/y6pb9bjg

Herausforderungen: 
Server-Side-Rendering
Einrichtung/Configuration (IndexDBs: Pouch-DB, Dexie, RxDB, ..)

## PWA Konzepte
Bietet Vorteile von Nativen Apps und Web Apps.
Es müssen grundsätzlich immer Ausweichmechanismen geprüft bzw. realisiert werden, damit die Anwendung bei PWA Problemen noch "normal" arbeitet (z.B. Quotas bei verwendung von IndexedDB, Nutzung der IndexedDB nicht zugelassen).

### Progressive Enhancements nutzen/unterstützen:
Offline, Caching + versetze Kommunikation mit dem Server, Home Screen, push Notification etc.



#### Offline

#### Caching
- Readonly
- Read-Write mit Synchronisierung
- 

##### Cache Strategien
- Chache Only 
- Network only 
- Try Cache, fallback Network
- Try Network, fallbach Cache
- ...

Cachgröße: Hängt vom Browser ab, wird i.R. aber automatisch geregelt

caches.open(NAME).then(..
caches.match(request).then(..

http://tinyurl.com/pwa-caching

#### Home Screen


### Service Worker (SW)
- Kommunikation mit dem Server
- Push-Nachrichten
- Entscheidung ob Daten aus Cache oder vom Server geladen werden.
- Jede App hat einen eigenen Service Worker und keinen Zugriff auf andere SW (Same Origin Policy)

node-module/sw-toolbox ... (heute workbox)

Wird von Browser aus der Anwendung geladen. Wird beim Start der Anwendung auf aktualisierung geprüft. (Nicht einfach löschbar, müsste mit einem Pass-Through-SW (der also alles durchreicht) ersetzt werden.

Im Chrome F12 -> Application -> aktive SW und Storage zu sehen.

#### Scope
- / gesame Anwendung
- leer -> Verzeichnis wo SW liegt

### Daten lokal speichern

Nicht Caching, sondern Datenspeicherung.
https://developers.google.com/web/fundamentals/instant-and-offline/web-storage/offline-for-pwa

Quota-überschreitungen müssen von Anwendung selbst überwacht/abgefangen werden.

- localStorage (kein Zugriff über SW)
- WebDb (deprecated)
. IndexedDb (NoSQL DB, existiert bei Nutzung pro Anwendung (evtl. auch mehrere))

Kein langfristiger Offlinebetrieb, überbrücken bei Unterbrechungen.

Leeren der IndexedDB - wird nicht beim normalen leeren des Browsercaches mitgelöscht. Muss i.d.R. separat angehakt werden.

#### Wrapper/Abstaktionen für Indexed-DB
PouchDB (kann als Pendent zu CouchDB genutzt werden und damit synchronisierung)
Dexie (bequem)
RxDB (unterstützt auch Observables -> Asynchrone information)

##### Dexie
npm i dexie --save


#### Push Notifactions 
Dazu werden vom Browser Pakete bereitgestellt, die zum Server geschickt werden müssen um das Ziel der Push-Notification festzulegen.
Hier können gespeicherte Daten nach dem Push aktualisiert werden?;?
Das Paket lässt sich jeder Browser individuell von seinem eigenen Push-Service liefern.
Verbindungsdauer kann Browser mit Push-Service ausmachen.

! Push-Notification -> Kommen auch wenn App verlassen wurde aber Browser noch geöffnet ist. (! Internet-Cafe) 

### Home Screener
- Über das Web App Manifest 

''' {
''' "name": "Hotel PWA-Demo",
''' "short_name": "Hotel",
''' "icons": [{
''' "src": "images/touch/icon-128x128.png",
''' "sizes": "128x128",
''' "type": "image/png"
''' }, [...] ],
''' "start_url": "/index.html?homescreen=1",
''' "display": "standalone",
''' [...]
''' }

Referenziern:
''' <!-- Web Application Manifest -->
''' <link rel="manifest" href="manifest.json">

start_url mit hinweis, dass es vom homescreen gestartet wurde.

Geht seit 2018 auch bei Apple.

### Background Sync
* App fordert Background Sync an
* Service Worker führt Sync Event
aus, wenn es passend erscheint
(Netzwerk, Akku, ...)

aktuell nur von Chrome und Opera unterstützt (caniuse.com)

## PWA Live
HIgh-Level APIs von Google
- Workbox
- @Angular/service-worker

### Angular/service-worker
schlanke eingeschränkte service-worker
Im Appmodule in die Imports 
'''ServiceWorkerModule.register('/ngsw-worker.js',
'''    {enabled: environment.production}),

Service Worker killen -> F12 -> Application -> beim Service Worker: unregister

Status des SW feststellen  URL/ngsw/state

### Angular/pwa
installiert auch angular/service-worker

ng add @angular/pwa
-> appModule -> hier serviceworker ngsw-worker.js "deklaration"; Bei eigenem SW wäre der so ähnlich anzugeben.

nur nach prodbuild verfügbar - vermutlich eher wegen kollisionpotential mit debugging

Um Dateien/Daten updaten existiert ein update service. Dieser vergleicht Hash-Werte seiner gecachten Dateien mit den entsprechenden auf dem Server -> Neu laden wenn unterschied existiert. Muss man in der Regel beim Start machen.


#### config
assetgroups <- für app dateien
installmode: prefetch, lazy,..
updatemode: prefetch

datagroups  <- für daten


## Sonstige:
http-server -o (Server Starten)
live-server
self = Service Worker (var context: any = self) 
  context.skipwaiting() <-- sofort starten, sonst erst nach Browserneustart.
Cookies : HTTPOnlyCookies.
Angular (@angular.service-worker) abstrahiert von der Low-Level API. Genauere Definitionen => Methoden selbst schreiben.
Bei uns Problem mit dem aktualisieren der Index.html -> die dann nicht mehr zum gespeicherten Hashwert passt über den die Synchronisierung mit geänderten Daten funktioniert ?.
Development Ansicht -> zur Ansicht von JavaScript log -> in Console "All levels" anzeigen bzw. Verbose anzeige aktivieren.

### Über Chrome pwa auf Desktop bringen
chrome://flags/#enable-desktop-pwas
Bei der App (drei punkte) Installieren. 

### push simulieren
Node Script um Push Notifikatation zu simulieren: npm i web-push --save-dev
  const pushSubscription = { "endpoint":"[...]", [...] }
 Starten mit node send-push.js  ( das JS File muss angelegt sein )
 
 const webpush = require('web-push');

const options = {
    vapidDetails: {
        subject: 'http://127.0.0.1:8080',
        publicKey: 'BBc7Bb5f5CRJp7cx19kPHz5d9S5jFSzogxEj2V1C44WuhTwd78tnXLPzOxGe0bUmKJUTAMemSKFzyQjSBN_-XyE',
        privateKey: 'tBoppvhj9A9NO0ZrFsPiH_CoAZ84TagjxiKyGjR4V8w'
    },
    TTL: 5000
}

const pushSubscription = {"endpoint":"https://fcm.googleapis.com/fcm/send/eLOELq9SYW8:APA91bE3VxAVe1yzum7Q3w8mRzIg6agX1yDzEEiTAf50KGGdnqMEisaGdGiC8B3_OyIrNXtAQmFLlyuWvEGQlHVIV-6P3vnXLjrbWeUHYzjkRuUplX7DKNk3VbOlF408aeMjiZHLktQK","expirationTime":null,"keys":{"p256dh":"BLkNGI_GYTnJPQ84XZuB9SyI1P98mbMxy-1PbbLA4ixHTeMdo0P1UucEfrDxiD19biFfWqnCcHhD232PbxggMZg","auth":"5ubjl_JeMT31_zZE40dFfg"}};

const payload = JSON.stringify({
    notification: {
        title: 'Your Gate Changed',
        body: 'Your Gate is now G62',
        icon: './assets/bed.png',
        data: 'additional data'
    }
});

webpush.sendNotification(
    pushSubscription,
    payload,
    options
);

 ## Lösung PWA SSR Problem
 Eigenen Service Worker implementieren, der auch den ngsw-server (oder war's worker) der für die index.html zuständig ist.
 Dieser 
 
 Manfred Steyer aktualisiert sein Beispiel und sendet die url auf sein eingechecktes Beispiel. (git: manfredsteyer/pwa_2019_02_19)
 
 
