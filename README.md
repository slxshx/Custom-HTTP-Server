# 🚀 Fahrplan: Custom HTTP-Server (C#)

Dieses Dokument dient als technische Leitlinie für die Implementierung eines eigenen HTTP-Stacks. Jede Zeile Code wird manuell erstellt, um das Protokoll von Grund auf zu verstehen.

## Architektur-Komponenten

### 1. HttpServer.cs
**Ziel:** Das "Netzwerk-Fundament".
*   Verwaltet den `TcpListener`.
*   Akzeptiert eingehende Verbindungen in einer Endlosschleife.
*   Übergibt den `TcpClient` an den `HttpProcessor`.
*   **Fokus:** Muss asynchron arbeiten (`Task`), damit der Server nicht blockiert.

### 2. HttpProcessor.cs
**Ziel:** Die "Schaltzentrale".
*   Liest die rohen Bytes aus dem `NetworkStream`.
*   Initiiert das Parsing durch die `HttpRequest`-Klasse.
*   Entscheidet, welche Antwort (z. B. 200 OK oder 404) gesendet werden soll.
*   Schreibt die finale Antwort der `HttpResponse`-Klasse zurück in den Stream.

### 3. HttpRequest.cs
**Ziel:** Der "Dolmetscher" für eingehende Daten.
*   Konvertiert den Bytestream/String in ein logisches C#-Objekt.
*   **Parsing-Logik:** Extrahiert die Methode (GET/POST), den Pfad (`/index`) und die Header.
*   **Erkennungsmerkmal:** Sucht nach der doppelten Leerzeile `\r\n\r\n`, um Header vom Body zu trennen.

### 4. HttpResponse.cs
**Ziel:** Der "Konstrukteur" der Antwort.
*   Baut das Paket nach dem HTTP-Standard zusammen.
*   Verantwortlich für die korrekte Struktur: `HTTP-Version Status-Code Status-Text`.
*   Berechnet Header wie `Content-Length` und setzt den `Content-Type`.
*   Wandelt alles für den Versand zurück in ein `byte[]`.

---

## Meilensteine (Milestones)

### 🚩 Milestone 1: Die Verbindung steht
*   Der `TcpListener` ist an Port 8080 gebunden.
*   Dein Programm gibt "Client verbunden" aus, wenn du `localhost:8080` im Browser aufrufst.

### 🚩 Milestone 2: Den Browser verstehen
*   Der `HttpProcessor` liest den Stream komplett aus.
*   Du siehst den Text `GET / HTTP/1.1...` ungefiltert in deiner Konsole.

### 🚩 Milestone 3: Logik-Extraktion
*   Die `HttpRequest`-Klasse kann den Pfad isolieren.
*   Konstruiere eine Konsolenausgabe: `Route: /mein-pfad wurde angefragt`.

### 🚩 Milestone 4: Die erste Webseite
*   Der Browser erhält eine valide Antwort von `HttpResponse`.
*   Du siehst eine echte HTML-Seite im Browser anstatt einer Fehlermeldung.

---

## Technische Anforderungen & Specs

*   **Standard:** HTTP/1.1 gemäß **RFC 9112**.
*   **Trennzeichen:** Das Protokoll verlangt zwingend `\r\n` (CRLF) nach jeder Header-Zeile.
*   **Encoding:** Ausschließlich `UTF-8` verwenden.
*   **Werkzeuge:** Wireshark zur Paket-Analyse und die Browser-DevTools (F12).
