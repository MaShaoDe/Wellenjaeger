# Wellenjäger: Konzept (Stand 2026-11-12, Entwurf)

SDR-Empfänger für Kurzwelle als Workshop für Prüfungskandidaten im Amateurfunk. Gemeinsam bauen, mit Software aus bestehenden offenen Projekten. Eigene Platine, 3D-gedrucktes Gehäuse. Open Source.

Grundsatz: Bestehende offene Software forken und anpassen, nicht von Grund auf neu entwickeln. Die Hardware (Platine, Gehäuse) ist ein eigenes Design. Als Mikrocontroller dient ein ESP32-S3-DevKitC-1 (N16R8) aus dem Vorrat von Marcel, wie beim Funkfüürli.

## 1. Funktion
Blockschaltbild: Antenne, Bandfilter, Tayloe-Mischer (Quadratur), Si5351 als Oszillator, I2S-Audio-Codec, ESP32-S3 (FFT, Demodulation SSB/AM/CW), Display und Encoder, Audioausgang, optional Handy per Wi-Fi.

* Nur Empfang, dafür ist keine Amateurfunk-Lizenz nötig.
* Bänder: Start mit 40 m und 20 m, später 15 m und 10 m. Pro Band ein Bandfilter (Platinenvariante oder Steckfilter).
* Kein UKW-Rundfunk: Der Tayloe-Aufbau deckt KW bis ca. 30 MHz ab. UKW hört man mit dem Funkfüürli.
* Wasserfall: auf dem Gerät und/oder als Webseite vom ESP32 (eigenes WLAN, WebSocket, kein Internet nötig). BLE ist dafür zu langsam und nur als Fernbedienung geeignet.
* Schwierigkeit: mittel bis hoch (RF, Abgleich, I/Q-Balance). Zielgruppe mit Grundkenntnissen.

## 2. Software-Vorbilder und Lizenzstand
Repos am 2026-11-12 geprüft.

| Projekt | Lizenz im Repo | Folge |
|---|---|---|
| michalin/easysdr | GPL-3.0 | Fork möglich, Ableitung muss ebenfalls GPL-3.0 sein |
| jgauchia/ESP32-SDR | keine Lizenzdatei | Ohne Lizenz sind alle Rechte beim Autor. Vor Nutzung beim Autor anfragen |
| thaaraak/ESP32-A1S-Tayloe | keine Lizenzdatei gefunden | Wie oben: anfragen. KiCad-Dateien und Code als Anschauung nutzbar, nicht kopieren |

Wichtigste Basis laut Lizenz: easysdr (GPL-3.0). Der Rest dient als Anschauung, bis die Autoren geantwortet haben.

* jgauchia/ESP32-SDR: ESP32 mit TFT, PCM1808, PCM5102 und Si5351, mit FFT-Bibliothek. Kommt dem geplanten Aufbau am nächsten.
* thaaraak/ESP32-A1S-Tayloe: Tayloe-Mischer mit KiCad-Dateien und Videos.
* michalin/easysdr: Tayloe-Empfänger bis ca. 30 MHz mit Si5351.

## 3. Mikrocontroller
ESP32-S3: genug Leistung für 1024-Punkte-FFT bei 48 kHz Bandbreite plus Demodulation. Stromaufnahme ca. 40 bis 60 mA, vor der Festlegung messen.

Die ESP32-S3 und C3 haben nur BLE und kein Bluetooth Classic (A2DP), im Datenblatt von Espressif prüfen. Soll der Ton direkt per Bluetooth zum Lautsprecher, gibt es zwei Wege:
* kleines externes Bluetooth-Sendemodul auf der Platine (funktioniert mit dem S3), oder
* klassischer ESP32 (z.B. ESP32-A1S) mit A2DP in der Firmware, wie im Referenzprojekt ESP32-A1S-Tayloe.

## 4. Audio
Gleiches Konzept wie beim Funkfüürli:
1. Interner Lautsprecher mit sparsamem Class-D-Verstärker.
2. 3.5-mm-Buchse für Kopfhörer, schaltet den Lautsprecher beim Einstecken ab.
3. Zusätzlich ein Line-Out für Stereoanlage oder PC.
4. Bluetooth zu einem Lautsprecher als Option (Latenz ca. 100 bis 200 ms, beim Abstimmen und bei CW spürbar, Kabel bleibt die Referenz).

## 5. Störarmes Design
Beim Wellenjäger sind HF-Teil, Digitalteil und Audio sauber zu trennen:
* Schirmkappe über Mischer und Filter (Weissblech, Kupferklebeband oder aufgelötete Kappe), nicht über das Druckmaterial.
* Saubere Versorgung des Analogteils: Akku oder Netzteil, Ferrit und Kondensator, dann LDO. Schaltregler nur mit LC-Filter.
* Durchgehende Massefläche, Digital- und HF-Teil getrennt, kurze Leitungen.
* Wie beim Funkfüürli: Laden und Hören nicht gleichzeitig. Schaltregler und MPPT-Solarregler stören im KW-Bereich.

## 6. Eigene Platine
* Design in KiCad 9, Gerber im Repo, Fertigung bei JLCPCB oder PCBWay.
* Trägerplatine mit Buchsenleisten für das ESP32-Modul. Die Pinbelegung vor dem Layout gegen das Datenblatt prüfen.
* Bestückung (Vorschlag): SMD-Teile (Si5351, Codec, Mischer-IC) werksseitig bestücken lassen. Encoder, Buchsen, Spulen, ESP32-Modul und Antennenanschluss werden im Workshop von Hand gelötet.
* Bandfilter als Teil der Platine oder als Steckmodule pro Band.

## 7. Gehäuse (3D-Druck im Funkamateur-Club oder FabLab)
* Robustheit, Last und Falltest: siehe Abschnitt 8. Keine starren Rippen von oben auf Modul oder Platine.
* Gleiche Vorgaben wie beim Funkfüürli: höchstens ca. 180 x 120 mm Grundfläche, ohne Stützmaterial druckbar, PETG, M3-Schrauben mit Gewindeeinsätzen, parametrisches CAD im Repo.
* Grösser, mit Platz für Display (Wasserfall), Encoder, Lautsprecher und Buchsen (Antenne, Kopfhörer/Line-Out, USB).
* Interne Trennwand oder Aufnahme für die Schirmkappe über dem HF-Teil.
* Lüftung und Platz für Erweiterungen (weitere Bandfilter, Pegelanpassung).
* Kein Rufzeichen und keine privaten Angaben im Gehäuse.

## 8. Robustheit
Der Wellenjäger ist ein Gerät für Club, Werkstatt und Feld und soll Transport und Stürze aushalten.

Abnahmekriterium: **Falltest aus 1 m auf Holz oder Fliesen.** Mehrere Stürze (flach auf Vorder- und Rückseite, auf eine Kante, auf eine Ecke, mit ausgefahrener Antenne). Danach muss gelten: Gehäuse ohne Riss, nichts lose, Radio startet, Abstimmung, Display und Ton funktionieren, Akku sitzt fest, Laden geht. Das Ergebnis kommt in ein Protokoll (Datum, Gehäuse-Version, Material, Befund). Der Falltest ist zugleich ein Programmpunkt im Workshop.

Prinzipien:
1. **Last nie über die Platine.** Wenn jemand auf das Gerät sitzt oder steht, darf nichts auf Modul, Display oder Platine drücken. Keine starren Rippen oder Stege vom Deckel auf Modul oder Platine. Die tragende Struktur sind Gehäusewände und Stützsäulen neben der Platine.
2. **Weich halten statt hart klemmen.** Modul, Display und Akku werden mit flexiblem Kunststoff (TPU-Pads aus dem 3D-Drucker) oder Schaumstoff (Moosgummi, PU-Schaum) gepolstert und nur leicht angedrückt. Kein Dauerdruck auf Steckverbindungen.
3. **Modul:** Aufnahme mit Seitenführung im Gehäuse, von oben ein weiches Pad. Buchsenleisten mit gedrehten Präzisionskontakten statt einfacher Steckerstreifen. Für eine Endversion alternativ die Stiftleiste direkt einlöten.
4. **Platine:** 4 M3-Schrauben auf Abstandsbolzen, bei Bedarf mit Gummitüllen als Dämpfung.
5. **Gehäuse:** PETG, Wandstärke ca. 2.4 mm, abgerundete Ecken, Rippen nur an den Wänden, Stossecken (Bumper) aus TPU.
6. **Akku (schwerstes Teil):** im Halter verschraubt, im Fach seitlich geführt und gepolstert, mit Schraubdeckel. Geschützte Zelle. Das Gehäuse schützt die Zelle vor Quetschung.
7. **Das Gehäuse trägt die mechanischen Lasten, nicht die Lötstellen:** USB-C-Buchse mit enger Aussparung im Gehäuse abgestützt, BNC-Buchse mit Mutter durch die Gehäusewand verschraubt, Teleskopantenne am Gehäuse verankert, Encoder und Klinkenbuchse mit Mutter an der Frontplatte. Die Lötstellen tragen nur die Elektrik.
8. **Verkabelung:** Steckverbinder mit Zugentlastung, kurze Leitungen, keine frei hängenden Kabel.
9. **Bauteile mit Kleber sichern:** grosse Bauteile (Elkos, Spulen, Ferritstab, Steckverbinder) bekommen einen kleinen Tropfen Kleber. Welcher Kleber, ist offen:
   * Heissleim (einfach, am besten Niedertemperatur-Sorte, weicht aber bei ca. 60 bis 80 Grad auf, z.B. im Auto in der Sonne)
   * neutral vernetzendes Silikon (flexibel, reparierbar). Essigsäure-vernetzendes Silikon nicht verwenden, es korrodiert Elektronik.
   * Epoxidharz (hart, schwer zu reparieren)
   Kriterien: reparierbar, temperaturfest, nicht leitend, kindgerecht zu verarbeiten. Entscheidung nach Test.
10. **Firmware:** Der ESP32 hat eingebaute Hardware-Watchdogs. Watchdog und Brown-out-Erkennung aktivieren, bei Hänger von Prozessor oder I2C-Bus zum Si4732 automatisch neu starten, danach letzte Frequenz und Einstellungen wiederherstellen. Prüfen, was der ATS-Mini-Fork davon schon mitbringt.
11. **Zusätzlich beim Wellenjäger:** Schirmkappe über dem HF-Teil und die Steckmodule für die Bandfilter mechanisch sichern. Die grössere Platine bekommt mehr Befestigungspunkte.

## 9. Lizenzen
* Hardware CERN-OHL-P-2.0, Dokumentation CC-BY-4.0.
* Firmware: richtet sich nach dem Fork (easysdr: GPL-3.0).

## Offene Punkte
1. Autoren von ESP32-SDR (jgauchia) und ESP32-A1S-Tayloe (thaaraak) wegen Lizenz anfragen. Sonst easysdr als Basis.
2. Bandfilter für 40 m und 20 m auslegen, später 15 m und 10 m.
3. Bluetooth-Audio: externes Sendemodul oder klassischer ESP32 mit A2DP?
4. Modultyp des ESP32-S3 festlegen (Pinbelegung, Flash/PSRAM).
5. Bestückung: Werk oder Handarbeit.
6. Kursgrösse und Dauer.
7. Welcher Drucker und welcher Bauraum im Club und im FabLab?
8. Namensprüfung (Swissreg/IGE und GitHub) vor der Veröffentlichung.
9. Reihenfolge: Funkfüürli zuerst (einfacher), Wellenjäger als zweiter Kurs.
10. Kleber für Bauteile festlegen (Heissleim, neutral vernetzendes Silikon, Epoxid).
11. Watchdog und Neustart in der Firmware: Was bringt der ATS-Mini-Fork schon mit?
12. Falltest aus 1 m: Protokollvorlage und Testplan schreiben.

## Quellen
* GitHub: michalin/easysdr (GPL-3.0), jgauchia/ESP32-SDR und thaaraak/ESP32-A1S-Tayloe (beide ohne Lizenzdatei)
