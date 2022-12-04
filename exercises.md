# Übungen zu S3/Minio

1. **Erstellen Sie einen Fork von diesem Repository!**
2. **Klonen Sie Ihren Fork, nicht das Original-Repository!**
3. **Reichen Sie Ihre Lösungen per Pull Request ein!**

Bei jeder Aufgabe ist angegeben, in welcher Datei Sie welche Inhalte ablegen
müssen.

## Aufgabe 1: Minio-Server verwenden

Der Minio-Server (`minio`) und -Client (`mc`) sind vorinstalliert.

### Datenverzeichnis erstellen

Erstellen Sie ein Verzeichnis in Ihrem `$HOME`-Verzeichnis namens `minio-data`:

    $ mkdir ~/minio-data

### Benutzername und Passwort konfigurieren

Editieren Sie anschliessend die Datei `~/.bashrc` mit einem Texteditor Ihrer
Wahl. (Falls Sie die Datei mit einem grafischen Dateiauswahl-Dialog öffnen,
müssen Sie versteckte Dateien einblenden.)

Definieren Sie unten an der Datei zwei neue Umgebungsvariablen mit dem
`export`-Befehl:

    export MINIO_ROOT_USER=minio
    export MINIO_ROOT_PASSWORD=topsecret

Speichern Sie die Datei ab. Laden Sie die Datei anschliessend mit dem
`source`-Befehl nach:

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

Besuchen Sie nun die Seite [localhost:9000](https://localhost:9000). Sie werden
zu einem Login-Bildschirm weitergeleitet.

Loggen Sie sich mit dem zuvor definierten Benutzernamen und Passwort ein.

Sie sollten nun die leere Bucket-Übersicht sehen.

### Bucket erstellen

Erstellen Sie nun einen neuen Bucket per Klick auf die entsprechende
Schaltfläche ("Create Bucket") oben rechts. Nennen Sie diesen "hello". Sie
brauchen keine anderen Optionen anzuwählen. Klicken Sie anschliessend auf
"Create Bucket", um den neuen Bucket definitiv zu erstellen.

### Datei hochladen

Betätigen Sie nun die "Upload"-Schaltfläche oben rechts. Laden Sie nun **drei**
Dateien unterschiedlichen Typs aus Ihrem `$HOME`-Verzeichnis hoch:

1. Eine Bilddatei (z.B. eine aus dem `pics`-Unterverzeichnis)
2. Eine Textdatei (z.B. diese hier)
3. Eine Binrdatei (z.B. `mc` aus dem Verzeichnis `~/.local/bin`)

### Kontrollfragen

Legen Sie die Antworten auf die folgenden Fragen in `aufgabe-1.md` ab, und fügen
Sie die Datei diesem Git-Repository hinzu.

1. Wie gross sind die drei Dateien gemäss Anzeige im Web-Interface (grob
   zusammengerechnet)?
2. Wie gross ist der `hello`-Bucket auf dem Dateisystem unter `~/minio-data`?
   Verwenden Sie den Befehl `du -hs` um die Grösse zu ermitteln!
3. Betrachten Sie die Dateien und Ordner im Verzeichnis `~/minio-data/hello`.
   Wie sind die Daten organisiert, und warum ist das wohl so gelöst?

## Aufgabe 2: Minio-Client verwenden

Minio stellt einen Kommandozeilen-Client zur Verfügung, mit dem die Daten auf
dem Minio-Server komfortabel verwendet werden können.

### Client-Alias erstellen

Der Minio-Client `mc` kann so konfiguriert werden, dass er unter einem
Alias-Namen mit einem bestimmten Minio-Server zusammenarbeitet. Führen Sie den
folgenden Befehl aus, um einen neuen Alias namens `local` zu erstellen:

    $ mc alias set local http://localhost:9000 minio topsecret

Unter dem Namen `local` kann man nun auf die lokale Minio-Instanz zugreifen.
Testen Sie den Zugriff mit dem folgenden Befehl:

    $ mc admin info local

Es sollten nun Zustandsinformationen zum lokalen Minio-Server angezeigt werden.

Die Konnektivität kann mithilfe des `ping`-Befehls überprüft werden:

    $ mc ping local

Mit `[Ctrl]-[C]` stoppen Sie die `ping`-Endlosschleife.

### Client-Befehle kennenlernen
    
Der Minio-Client `mc` bietet zahlreiche Befehle, die man von Unix/Linux her kennt:

- `ls`: Dateien auflisten
- `mv`: Dateien umbenennen (verschieben)
- `rm`: Dateien entfernen (löschen)
- `cat`: Dateien zusammenhängen/ausgeben
- `cp`: Dateien kopieren
- `head`: Anfang einer Datei ausgeben
- `find`: Dateien suchen
- `diff`: Dateien vergleichen
- `du`: Dateigrösse ermitteln
- `stat`: Metadaten zu Dateien und Verzeichnissen ausgeben

Diese Befehle können zwar für normale Dateien im Dateisystem verwendet werden.
Die `mc`-Versionen dieser Befehle bieten aber dadurch keinen Vorteil gegenüber
den herkömmlichen Befehlen.

**Die `mc`-Befehle können jedoch auf S3-Daten angewendet werden, indem man Alias
und Bucket (`[Alias]/[Bucket]`) angibt:**

    $ mc ls local/hello
    [2022-12-04 13:31:53 CET] 346KiB STANDARD minio-logo.png
    [2022-12-04 13:32:07 CET] 2.0KiB STANDARD exercises.md
    [2022-12-04 13:32:59 CET]  24MiB STANDARD mc

    $ mc head -n 1 local/hello/exercises.md
    # Übungen zu S3/Minio

Weiter bietet `mc` einige Befehle, die nur im Zusammenhang mit einem S3-Storage
sinnvoll sind. Die folgenden Befehle dienen dem Umgang mit Buckets und
S3-Datenobjekten:

- `mb`: Bucket erstellen
- `rb`: Bucket entfernen
- `tag`: Tags verwalten
- `pipe`: Standardausgabe in Objekt umleiten
- `mirror`: Daten mit anderem S3-Server synchronisieren
- `retention`: Speicherdauer von Objekten festlegen
- `share`: Objekte per URL teilen
- `version`: Versionierung von Buckets verwalten

### Bucket via `mc` erstellen

Erstellen Sie einen neuen Bucket namens `backup` mithilfe des `mc mb`-Befehls.
Kopieren Sie dann wieder drei unterschiedliche Dateien mit dem `mc cp`-Befehl in
den neuen Bucket.

Kontrollieren Sie anschliessend im Browser, ob der Bucket erstellt worden ist
und alle Dateien enthält.

Führen Sie nun den Befehl `mc ls local/backup` aus und leiten Sie dessen Ausgabe
in die Datei `aufgabe-2-ls.txt` weiter, die Sie diesem Git-Repository hinzufügen.

### Buckets taggen

Verwenden Sie den `mc tag`-Befehl, um den Buckets die folgenden Tags zu
vergeben:

| Bucket   | Tag       |
|----------|-----------|
| `hello`  | `upload`  |
| `backup` | `archive` |

Mit `mc tag --help` erhalten Sie Hilfestellungen dazu.

Listen Sie nun die Tags mit `mc tag` **im JSON-Format** auf, und speichern Sie
die Ausgaben unter den Dateien `aufgabe-2-tag-hello.json` und
`aufgabe-2-tag-backup.json` ab. Fügen Sie beide Dateien diesem Git-Repository
hinzu.

## Aufgabe 3: s3cmd

Im Gegensatz zu `mc` ist der Befehl `s3cmd` nicht nur für die Zusammenarbeit mit
MinIO ausgelegt, sondern sollte auch mit anderen S3-Implementierungen und
-Angeboten zusammenspielen.

Installieren Sie `s3cmd` mit dem folgenden Befehl:

    $ sudo apt install -y s3cmd

Legen Sie nun die folgende Konfiguration unter `~/.s3cfg` an:

    host_base = localhost:9000
    host_bucket = localhost:9000
    use_https = False
    access_key = minio
    secret_key = topsecret

Listen Sie nun die Dateien im `hello`-MinIO-Bucket auf:

    $ s3cmd ls s3://hello

Mit `s3cmd` müssen Sie anstelle des Alias `local/` bloss das Protokoll `s3://`
als Präfix angeben.

Erstellen Sie mit `s3cmd mb` nun einen neuen Bucket namens `backup-copy`:

    $ s3cmd mb s3://backup-copy

Kopieren Sie die drei Dateien aus dem `backup`-Bucket in den
`backup-copy`-Bucket. Verwenden Sie dazu den `s3cmd cp`-Befehl.

Dokumentieren Sie die drei Befehle in der Datei `aufgabe-3.txt`, welche Sie
diesem Git-Repository hinzufügen.

Kontrollieren Sie nun im MinIO-GUI im Browser sowie per `s3cmd ls`-Befehl, ob
alle Dateien erfolgreich kopiert worden sind.

## Aufgabe 4: s3fs

FUSE (Filesystem in Userspace) bietet die Möglichkeit, Dateisysteme ohne
Root-Berechtigungen einzubinden.

Mithilfe von `s3fs` können S3-Buckets als normale Ordner eingebunden werden.

Installieren Sie `s3fs` folgendermassen:

    $ sudo apt install -y s3fs

Erstellen Sie eine Konfigurationsdatei `~/.passwd-s3fs` mit dem folgenden
Inhalt:

    minio:topsecret

Ändern Sie die Berechtigungen für diese Datei folgendermassen:

    $ chmod 600 ~/.passwd-s3fs

Dadurch stellen Sie sicher, dass nur Ihr Benutzer diese Datei lesen und
schreiben kann (`s3fs` verweigert sonst den Dienst).

Erstellen Sie nun mit dem `mkdir`-Befehl ein Verzeichnis `~/minio-mount` mit
Unterverzeichnissen für die verschiedenen Buckets:

    $ mkdir -p ~/minio-mount/hello
    $ mkdir -p ~/minio-mount/backup
    $ mkdir -p ~/minio-mount/backup-copy

Mithilfe des `s3fs`-Befehls können die einzelnen Buckets nun "gemounted", d.h.
eingehängt werden:

    $ s3fs hello ~/minio-mount/hello -o use_path_request_style,url=http://localhost:9000
    $ s3fs backup ~/minio-mount/backup -o use_path_request_style,url=http://localhost:9000
    $ s3fs backup-copy ~/minio-mount/backup-copy -o use_path_request_style,url=http://localhost:9000

Die Unterverzeichnisse von `~/minio-mount/` können nun weitgehend wie lokale
Ordner verwendet werden.

Kopieren Sie nun eine beliebige Datei, die noch nicht in MinIO vorhanden ist, in
alle drei Buckets über das `~/minio-mount/`-Verzeichnis.

Überprüfen Sie nun im MinIO-GUI im Browser, ob Sie die Datei in jedem Bucket
sehen können.

Führen Sie nun den folgenden Befehl aus:

    $ mount | grep minio

Speichern Sie die Ausgabe in der Datei `aufgabe-4.txt` ab.

Damit wird demonstriert, dass `s3fs` die S3-Buckets über das Betriebssystem
(d.h. über eine Kernel-Funktion) einhängt.

Mit dem `umount`-Befehl können die Buckets wieder vom Dateisystem gelöst werden:

    $ umount ~/minio-mount/hello/
    $ umount ~/minio-mount/backup
    $ umount ~/minio-mount/backup-copy/

(Zwar gibt es hierfür auch den `s3fs unmount`-Befehl, es ist aber bemerkenswert,
dass dies auch mit dem gewöhnlichen `umount`-Befehl funktioniert.)

Mit `find` können Sie nun überprüfen, dass keine Dateien mehr "da" sind, sondern
nur noch die Verzeichnisse:

    $ find ~/minio-mount
    minio-mount/
    minio-mount/backup-copy
    minio-mount/hello
    minio-mount/backup

Die Dateien existieren aber weiterhin in MinIO, was Sie gerne über den Browser,
per `mc ls` oder per `s3cmd ls` überprüfen können.

### Aufgabe 5: Einsatzgebiete

Sie haben mit MinIO und den Hilfsprogrammen `mc`, `s3cmd` und `s3fs` nun einige
Werkzeuge zum Umgang mit dem S3-Storage kennengelernt.

Überlegen Sie sich je einen Anwendungsfall für den privaten und den
professionellen Bereich (z.B. für Ihren Lehrbetrieb), und beschreiben Sie
diese beiden in der Datei `aufgabe-5.md`.

Gehen Sie dabei davon aus, dass nicht bloss ein lokaler Storage, sondern
derjenige von einem Public-Cloud-Anbieter verwendet wird, und dass Sie die
Region dafür frei wählen können.

### Zusatzaufgabe

Bearbeiten Sie Auftrag 5 und Auftrag 6 in
`auftrag-mengen-und-kostenabschaetzung.pdf` auf Seite 3 (siehe Unterlagen auf
Teams). (Auftrag 6 können Sie erst bearbeiten, wenn Sie das Mengengerüst
berechnet haben.)
