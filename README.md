**🚀 Fahrplan: Custom HTTP-Server (C#)
Dieses Dokument dient als Leitfaden für die Implementierung eines eigenen HTTP-Protokoll-Stacks auf TCP-Basis.
🏗 Architektur-Komponenten
1. HttpServer.cs
Verantwortung: Netzwerk-Management & Connectivity.
Aufgaben:
Initialisiert den TcpListener auf einem definierten Port (z.B. 8080).
Führt die Hauptschleife (while(true)) aus, um neue Verbindungen zu akzeptieren.
Delegiert jeden neuen TcpClient an den HttpProcessor (idealerweise asynchron).
Fokus: Stabilität und das Offenhalten der "Tür" für neue Anfragen.
2. HttpProcessor.cs
Verantwortung: Der "Workflow-Manager".
Aufgaben:
Liest die rohen Bytes aus dem NetworkStream des Clients.
Koordiniert den Datenfluss zwischen Parser (HttpRequest) und Antwort-Generator (HttpResponse).
Stellt sicher, dass der Stream nach der Antwort korrekt geschlossen wird.
Fokus: Orchestrierung der Logik.
3. HttpRequest.cs
Verantwortung: Datenstruktur & Parsing (Eingang).
Aufgaben:
Nimmt den rohen String/Byte-Stream entgegen.
Parsing-Logik: Zerlegt die Startzeile (GET /index HTTP/1.1).
Extrahiert Header-Felder in ein Dictionary<string, string>.
Erkennt das Ende des Headers durch die Leerzeile (\r\n\r\n).
Fokus: String-Manipulation und Datenspeicherung.
4. HttpResponse.cs
Verantwortung: Datenstruktur & Protokoll-Konformität (Ausgang).
Aufgaben:
Hält Informationen wie StatusCode (200, 404), ContentType und den Body.
Formatierung: Erzeugt einen gültigen HTTP-Antwort-String nach dem Muster:
HTTP/1.1 [Code] [Status]\r\n[Header]\r\n\r\n[Content]
Wandelt den finalen String zurück in ein Byte-Array für den Versand.
Fokus: Exakte Einhaltung des HTTP-Formats.
🚩 Meilensteine (Milestones)
Milestone 1: Der "Handshaker" 🤝
TcpListener startet erfolgreich.
Programm pausiert und wartet auf Verbindung.
Browser-Aufruf führt zu einer "Verbindung hergestellt" Meldung in der Konsole.
Milestone 2: Der "Listener" 👂
Der HttpProcessor liest den Stream aus.
Der komplette Browser-Request (GET...) wird 1:1 in der Konsole ausgegeben.
Wichtig: Korrektes Handling des Buffers beim Lesen der Bytes.
Milestone 3: Der "Interpreter" 🧠
Die Klasse HttpRequest kann den Pfad (URL) aus dem Request extrahieren.
Ausgabe in der Konsole: Anfrage empfangen für Pfad: /dein/pfad.
Milestone 4: Der "Speaker" 📢
HttpResponse baut eine minimale Antwort.
Browser zeigt nicht mehr "Verbindung unterbrochen", sondern "Hallo Welt" (oder eine HTML-Seite).
Königsdisziplin: Der Browser zeigt das Favicon-Icon korrekt an (da er danach separat fragt).
🛠 Technische Details für die Umsetzung
Feature	Vorgabe
Protokoll	TCP (Layer 4) für HTTP/1.1 (Layer 7)
Zeilentrennung	Immer \r\n (CRLF) verwenden
Encoding	System.Text.Encoding.UTF8
Testing	Browser (localhost:8080) & Wireshark
**
