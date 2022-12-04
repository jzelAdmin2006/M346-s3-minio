TODO: Einleitung mit Instruktionen

## Aufgabe 1: Minio-Server verwenden

Der Minio-Server (`minio`) und -Client (`mc`) sind vorinstalliert.

### Datenverzeichnis erstellen

Erstellen Sie ein Verzeichnis in Ihrem `$HOME`-Verzeichnis namens `minio-data`:

    $ mkdir ~/minio-data

### Benutzername und Passwort konfigurieren

Editieren Sie anschliessend die Datei `~/.bashrc` mit einem Texteditor Ihrer Wahl. (Falls Sie die Datei mit einem grafischen Dateiauswahl-Dialog oeffnen, muessen Sie versteckte Dateien einblenden.)

Definieren Sie unten an der Datei zwei neue Umgebungsvariablen mit dem `export`-Befehl:

    export MINIO_ROOT_USER=minio
    export MINIO_ROOT_PASSWORD=topsecret

Speichern Sie die Datei ab. Laden Sie die Datei anschliessend mit dem `source`-Befehl nach:

    source ~/.bashrc

Als Kurzschreibweise kann auch der `.`-Befehl verwendet werden:

    . ~/.bashrc

Geben Sie nun testhalber beide Variablen aus:

    $ echo $MINIO_ROOT_USER
    minio
    $ echo $MINIO_ROOT_PASSWORD
    topsecret

### Minio starten

Starten Sie nun den Minio-Server mit dem folgenden Befehl:

    $ minio server ~/minio-data

Lassen Sie den Server nun in diesem Terminal laufen.

### Auf Web-Interface einloggen

Besuchen Sie nun die Seite [localhost:9000](https://localhost:9000). Sie werden zu einem Login-Bildschirm weitergeleitet.

Loggen Sie sich mit dem zuvor definierten Benutzernamen und Passwort ein.

Sie sollten nun die leere Bucket-Uebersicht sehen.

### Bucket erstellen

Erstellen Sie nun einen neuen Bucket per Klick auf die entsprechende Schaltflaeche ("Create Bucket") oben rechts. Nennen Sie diesen "hello". Sie brauchen keine anderen Optionen anzuwaehlen. Klicken Sie anschliessend auf "Create Bucket", um den neuen Bucket definitiv zu erstellen.

### Datei hochladen

Betaetigen Sie nun die "Upload"-Schaltflaeche oben rechts. Laden Sie nun **drei** Dateien unterschiedlichen Typs aus Ihrem Home-Verzeichnis hoch:

1. Eine Bilddatei (z.B. eine aus dem `pics`-Unterverzeichnis)
2. Eine Textdatei (z.B. diese hier)
3. Eine Binrdatei (z.B. `mc` aus dem Verzeichnis `~/.local/bin`)

### Kontrollfragen

Legen Sie die Antworten auf die folgenden Fragen in `aufgabe-1.md` ab.

1. Wie gross sind die drei Dateien gemaess Anzeige im Web-Interface (grob zusammengerechnet)?
2. Wie gross ist der `hello`-Bucket auf dem Dateisystem unter `~/minio-data`? Verwenden Sie den Befehl `du -hs` um die Groesse zu ermitteln!
3. Betrachten Sie die Dateien und Ordner im Verzeichnis `~/minio-data/hello`. Wie sind die Daten organisiert, und warum ist das wohl so geloest?

## Aufgabe 2: Minio-Client verwenden

Minio stellt einen Client zur Verfuegung, mit dem die Daten auf dem Minio-Server komfortabel verwendet werden koennen.

### Ein Client-Alias erstellen

Der Minio-Client `mc` kann so konfiguriert werden, dass er unter einem Alias-Namen mit einem bestimmten Minio-Server zusammenarbeitet. Fuehren Sie den folgenden Befehl aus, um einen neuen Alias namens `local` zu erstellen:

    $ mc alias set local http://localhost:9000 minio topsecret

Unter dem Namen `local` kann man nun auf die lokale Minio-Instanz zugreifen. Testen Sie den Zugriff mit dem folgenden Befehl:

    $ mc admin info local

Es sollten nun Zustandsinformationen zum lokalen Minio-Server angezeigt werden.

Die Konnektivitaet kann mithilfe des `ping`-Befehls ueberpreuft werden:

    $ mc ping local

### Client-Befehle kennenlernen
    
Der Minio-Client `mc` bietet zahlreiche Befehle, die man von Unix/Linux her kennt:

- `ls`: Dateien auflisten
- `mv`: Dateien umbenennen (verschieben)
- `rm`: Dateien entfernen (loeschen)
- `cat`: Dateien auflisten (zusammenhaengen)
- `cp`: Dateien kopieren
- `head`: Anfang einer Datei ausgeben
- `find`: Dateien suchen
- `diff`: Dateien vergleichen
- `du`: Dateigroesse ermitteln
- `stat`: Metadaten zu Dateien und Verzeichnissen ausgeben

Diese Befehle koennen zwar fuer normale Dateien im Dateisystem verwendet werden, bieten aber dadurch keinen Vorteil gegenueber den herkoemmlichen Befehlen.

Die `mc`-Befehle koennen jedoch auf S3-Daten angewendet werden, indem man Alias und Bucket (`[Alias]/[Bucket]`) angibt:

    $ mc ls local/hello
    [2022-12-04 13:31:53 CET] 346KiB STANDARD arch-btw.png
    [2022-12-04 13:32:07 CET] 2.0KiB STANDARD exercises.md
    [2022-12-04 13:32:59 CET]  24MiB STANDARD mc

    $ mc head -n 1 local/hello/exercises.md
    TODO: Ausgabe dokumentieren

Weiter bietet `mc` einige Befehle, die nur im Zusammenhang mit einem S3-Storage sinnvoll sind. Die folgenden Befehle dienen dem Umgang mit Buckets und S3-Datenobjekten:

- `mb`: Bucket erstellen
- `rb`: Bucket entfernen
- `tag`: Tags verwalten
- `pipe`: Standardausgabe in Objekt umleiten
- `mirror`: Daten mit anderem S3-Server synchronisieren
- `retention`: Speicherdauer von Objekten festlegen
- `share`: Objekte per URL teilen
- `version`: Versionierung von Buckets verwalten

TODO: Uebungen ausdenken

## Aufgabe 3: s3cmd

    $ sudo apt install s3cmd

## Aufgabe 4: s3fs

    $ sudo apt install s3fs