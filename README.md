# Angular-PWA
Notizen etc. zu PWA

Fragen: 
Synchronisierung Cachng Browser <-> PWA


Download: http://tinyurl.com/y6pb9bjg

Herausforderungen: 
Server-Side-Rendering
Einrichtung/Configuration (IndexDBs: Pouch-DB, Dexie, RxDB, ..)

## PWA Konzepte
Bietet Vorteile von Nativen Apps und Web Apps.

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

- localStorage (kein Zugriff über SW)
- WebDb (deprecated)
. IndexedDb (NoSQL DB, 

Kein langfristiger Offlinebetrieb, überbrücken bei Unterbrechungen.

#### Wrapper/Abstaktionen für Indexed-DB
PouchDB (kann als Pendent zu CouchDB genutzt werden und damit synchronisierung)
Dexie (bequem)
RxDB (unterstützt auch Observables -> Asynchrone information)

#### Push Notifactions 
Dazu werden vom Browser Pakete bereitgestellt, die zum Server geschickt werden müssen um das Ziel der Push-Notification festzulegen.
Hier können gespeicherte Daten nach dem Push aktualisiert werden?;?

## PWA Live


## Sonstige:
http-server -o (Server Starten)
self = Service Worker (var context: any = self) 
  context.skipwaiting() <-- sofort starten, sonst erst nach Browserneustart.
b
