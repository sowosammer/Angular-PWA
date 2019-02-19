# Angular-PWA
Notizen etc. zu PWA

Fragen: 
Synchronisierung Cachng Browser <-> PWA


Download: http://tinyurl.com/y6pb9bjg

Herausforderungen: 
Server-Side-Rendering
Einrichtung/Configuration (Couch-DB)

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

#### Home Screen


### Service Worker (SW)
- Kommunikation mit dem Server
- Push-Nachrichten
- Entscheidung ob Daten aus Cache oder vom Server geladen werden.
- Jede App hat einen eigenen Service Worker und keinen Zugriff auf andere SW (Same Origin Policy)

node-module/sw-toolbox ...

#### Scope
- / gesame Anwendung
- leer -> Verzeichnis wo SW liegt



#### Push Notifactions 
Dazu werden vom Browser Pakete bereitgestellt, die zum Server geschickt werden müssen um das Ziel der Push-Notification festzulegen.

## PWA Live


## Sonstige:
http-server -o (Server Starten)
self = Service Worker (var context: any = self) 
  context.skipwaiting() <-- sofort starten, sonst erst nach Browserneustart.
b
