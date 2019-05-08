# Lerndokumentation Amacher Leandro





[TOC]



## K1

#### Umgebung Einrichten

Zuerst musste ich folgende Dinge installieren:

1. VirtualBox
2. Vagrant
3. Visualstudio-Code
4. Git-Client
5. SSH-Key für Client erstellt

Diese Installationen erfolgten Problemlos. Jedoch muss man beim SSH-Key noch ein paar zusätzliche Schritte machen welche nachfolgend dokumentiert sind.

#### Github kofigurieren

###### SSH-Key für Client erstellen

1. Terminal öffnen

2. Befehl mit dem Account-E-Mail von GitHub einfügen:

   ```
     $  ssh-keygen -t rsa -b 4096 -C "leandroamacher@gmail.com"
   ```

3. SSH-Key mit folgendem Befehl erstellen:

   ```
     Generating public/private rsa key pair.
   ```

4. Bei der Abfrage, unter welchem Namen der Schlüssel gespeichert werden soll, die Enter-Taste drücken (für Standard):

   ```
     Enter a file in which to save the key (~/.ssh/id_rsa): [Press enter]
   ```

5. Nun kann ein Passwort für den Key festgelegt werden. Ich empfehle dieses zu setzen und anschliessend dem SSH-Agent zu hinterlegen, sodass keine erneute Eingabe (z.B. beim Pushen) notwendig ist:

   ```
     Enter passphrase (empty for no passphrase): [Passwort]
     Enter same passphrase again: [Passwort wiederholen]
   ```

Jetzt kann man im Github Konto unter Settings > SSH und GPG keys > New SSH key öffnen und den SSH key eintragen. Dieser findet man in Windows unter %HOME%/.ssh/id_rsa.pub
Man kann ihn dort hinein kopieren.

![1552576466042](C:\Users\leam_\AppData\Roaming\Typora\typora-user-images\1552576466042.png)

###### Client konfigurieren

Man muss Git mit dem login vom GitHub-Account konfigurieren:

1. Terminal (*Bash*) öffnen

   ```
     $ git config --global user.name "<username>"
     $ git config --global user.email "<e-mail>"
   ```

###### Repository klonen

Das Repository klont man in ein lokales Verzeichnis auf dem Computer, damit man aus der Verzeichnisstrukur  besser arbeiten kann.
Man muss jedoch immer manuell über die Konsole die Dokumente auf GitHub laden.

1. Terminal (*Bash*) öffnen

2. Repository klonen:

   ```
     $ git clone https://github.com/mc-b/M300
   ```

3. In das M300-Verzeichnis wechseln:

   ```
     $ cd M300
   ```

4. Repository aktualisieren und Status anzeigen:

   ```
     $ git pull
   
     Already up to date.
   
     $ git status
   
     Your branch is up to date with 'origin/master'.
   ```

Nun kann man mit folgenden Befehlen jeweils arbeiten:

Git pull = Von GitHub in den Ordner welcher mit GitHub geklont wurde Dateien holen

Git push = Dateien von dem Ordner ins GitHub laden
Aktuell liegt der Ordner im C:\Documents\M300

Hier noch eine ausführlichere Anleitung: https://github.com/mc-b/M300/tree/master/10-Toolumgebung



## K2

#### GitHub Account erstellt

Für dieses Modul musste ich zuerst einen Account erstellen, da ich GitHub zuvor noch nie verwendet habe.

![1553028031007](C:\Users\leam_\AppData\Roaming\Typora\typora-user-images\1553028031007.png)



#### GitHub Client wurde verwendet

Um die Arbeit mit GitHub zu erleichtern, habe ich den GitHub Client für Windows lokal installiert.

 

#### Mark down

Alle meine Arbeitsschritte sowie die Punkte zur Erfüllung der LB01 werden hier drin festgehalten.

Als Mark down-Editor verwende ich Typora damit ich eine bessere Übersicht habe.

Das Mark down wurde von mir sinnvoll nach den Lernzielen strukturiert.



#### Persönlicher Wissensstand

Mein Wissenstand zu folgenden Punkten:

- Containerisierung

- Docker

- Microservices

  **Containerisierung:** Ich hatte bereits vor diesem Modul eine Schulung im eigenen Betrieb über die Containerisierung. Diese hat sich jedoch nicht spezifisch um Docker gehandelt wie es hier der Fall ist, sondern um das Konzept dahinter im allgemeinen. Den grössten Teil habe ich jedoch in diesem Modul gelernt und selbst angeeignet als ich im Internet darüber recherchiert habe.

  **Docker:** Docker habe ich bereits mehrmals gehört und ich wusste auch im groben für was man es benötigt jedoch nicht wie man es bedient. Die Komplexität und sämtliche Vorteile waren mir nicht bewusst. Ich konnte mir jedoch seit beginn der LB02 und über die Ferien gut Wissen aneignen.

  **Microservices:** Von Microservices hatte ich bereits vor dem Modul schon oft gehört und ich habe mich darüber im Internet schlau gemacht, da ich schon mehrmals hörte, dass dies eine zukünftige Technologie sein werde.





## K3

#### Bestehende Docker-Container kombinieren

Ich habe für die LB02 mehrere Services aufgesetzt. Als erstes setzte ich MySQL auf und erstellen eine Datenbank mit einem Benutzer. Anschliessend setze ich WordPress auf, welcher sich mit der vorher erstellten MySQL Datenbank verbindet.

Ich verwenden dafür Docker-Compose und mache Gebrauch von zwei verschiedenen Docker Images. Um unseren Service betreiben zu können muss logischerweise Docker Compose installiert werden.

#### Docker spezifische Befehle

- docker build -t "imagename"  buildet ein Docker Image aus einer Datei namens „*Dockerfile*“

- docker run -d -p 8080:80 "imagename"

    erstellt und startet einen Container basierend auf den Images,

  - -p  mappt den port 8080 (des Hostsystems) im Container auf den port 80.
  - -d  startet den Container im detached mode, der Container wird im Hintergrund gestartet und die console wird nicht von der Ausgabe des Docker Containers blockiert.

- docker images  zeigt alle gebauten oder heruntergeladenen Images an

- docker image rm IMAGE  entfernt das angegebene Image

- docker container ls -a  zeigt alle laufenden Container an, der Parameter `**-a**` zeigt auch alle derzeit nicht laufenden an. Dieser Command ersetzt `**docker ps**` welcher aber auch weiterhin noch funktioniert.

- docker start CONTAINER  startet den angegeben Container.

- docker logs -f (CONTAINER | SERVICENAME)

    zeigt den log output des Containers

  - -f : mit dem Parameter `**-f**` wird der folgende log output live ausgegeben.

- docker attach (CONTAINER | SERVICENAME)  hängt den Standard Input und Output auf die aktuelle Konsole. Man kann wie mit ssh eine Verbindung in den Container aufbauen. Achtung: dies ist keine richtige ssh Verbindung. Das merkt man insbesondere wenn man die Verbindung wieder auflösen will. `**CTRL-c**` oder exit stoppt auch den Container. Die Verbindung kann mit `**CTRL-p**` `**CTRL-q**` gelöst werden.

- docker exec -it CONTAINER /bin/sh  mit exec kann ein Befehl innerhalb des Containers ausgeführt werden. Wird der Befehl `**/bin/sh**` (wenn eine bash vorhanden ist kann auch `**/bin/bash**` verwendet werden) angegeben, ist dies eine alternative Variante um auf die shell im Container zuzugreifen. Wichtig ist, das die Option `**-it**` angegeben wird. Der Vorteil dieser Variante gegenüber docker attach ist, dass die shell wie gewohnt mit exit verlassen werden kann und der Container dabei nicht gestoppt wird.

- docker stop CONTAINER  stoppt den angegeben Container. Der Container wird heruntergefahren.

- docker kill CONTAINER  der Container wird mit einem Kill signal gestoppt.

- docker rm CONTAINER  entfernt den Docker Container (nur den Container nicht das Image)

#### Technische Angaben, Aufbau und Betrieb

In diesem Teil sind die Anforderungen dokumentiert.

Installation von Docker-Compose

Zuerst muss Docker-Compose nach `/usr/local/bin` mit den folgenden Befehl heruntergeladen werden:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Nachdem der Download abgeschlossen ist, muss man noch eine Berechtigung erteilen, um Compose ausführen zu können:

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

Nun kann man das Docker compose File erstellen (docker-compose.yml) und im Zielordner ablegen.

#### Docker-Compose File ausführen

Jetzt haben wir alles vorbereitet und können nun unser Docker-Compose File ausführen. Dies machen wir so:

```bash
docker-compose up
```

#### Port forwarding

Alle Ports gegen localhost müssen im Range 8080-8089 oder 3306 liegen.

Wie in der Anforderung genannt, habe ich unseren Docker Port 80 nach Guest 8080 geforwarded.

- Docker 80 => Guest 8080
  Prinzipiell muss der Service von ausserhalb der VM genutzt werden können.
  Da ich die für den Zugriff nötigen Port geforwarded habe ist unser Service von anderen Clients im selben Netz verfügbar.

### Dokumentation docker-compose.yml

In diesem Abschnitt ist mein Docker File dokumentiert.

#### docker-compose.yml

```bash
version: '3.3' #compose file version 

services:
 #DB / MYSQL Service
  db:
    image: mysql:5.7
    restart: always #immer neustarten des Containers
    volumes:
      - db_data:/var/lib/mysql #location wo es abgelegt wird
    environment:
      MYSQL_ROOT_PASSWORD: HaEsSiG53 #password von MYSQL
      MYSQL_DATABASE: wordpress #MYSQL Datenbank

#Wordpress Service
  wordpress:
    image: wordpress
    restart: always #immer neustarten des Containers
    volumes:
      - ./wp_data:/var/www/html #location wo es abgelegt wird
    ports:
      - "8080:80" #von Port 80 auf port 8080 weitergeleitet
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: SpielPlatz982
    depends_on:
       - db #wartet bis DB gestartet ist. Vorher wird es nicht gestartet!

volumes:
    db_data:
    wp_data:
```

#### 

#### Testprotokoll

| Nr.  | Testfall                                                     | Ergebnis                                                     | Positiv/Negativ |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------- |
| 1    | Befehl "docker-compose --version" eingegen damit wird sichergestellt, dass der Docker-Compose läuft. | Die Ausgabe lautet docker-compose version 1.23.1, build b02f1306. | Positiv         |
| 2    | VM ist Pingbar im Netzwerk um das Port forwarding sicher zu stellen. | Die VM konnte von einem anderen Gerät gepingt werden.        | Positiv         |
| 3    | MySQL Benutzer wurde für das Verwalten der Datenbank erstellt. | Es wurde ein MySQL Benutzer erfolgreich angelegt.            | Positiv         |
| 4    | MySQL Datenbank wird erstellt.                               | Die Datenbank wird beim Ausführen erfolgreich erstellt.      | Positiv         |
| 5    | Wordpress hat Zugriff auf die Datenbank.                     | Wordpress kann auf die MySQL Datenbank zugreifen und mit ihr kommunizieren. | Positiv         |



## K4

### Sicherheitsaspekte

Folgende Punkte wurden integriert:

- Berechtigungsmatrix der Benutzer
- Logs
- Port Forwarding
- Eigenes Netzwerk
- Starke Passwörter

#### Benutzer und Rechtevergabe

**Server**

| Benutzer | Funktion          | Rechte     |
| -------- | ----------------- | ---------- |
| root     | Admin unter Linux | Vollrechte |

**Datebank**

| Benutzer | Funktion                                       | Rechte         |
| -------- | ---------------------------------------------- | -------------- |
| root     | Admin unter Linux                              | Vollrechte     |
| mysql    | Benutzer von der MySQL Datenbank in Phpmyadmin | Execute rechte |

**Benutzer der Virtuellen Maschine**

| Benutzer | Funktion          | Rechte     |
| -------- | ----------------- | ---------- |
| lamacher | Normaler Benutzer | Read/Write |

#### Logs

Auf dem MySQL Server und auf Wordpress Server sind beides Logs vorhanden welche alle Aktivitäten aufzeigen.

#### Port forwarding

Damit von anderen Geräten darauf zugegriffen werden kann, habe ich wie bereits oben beschrieben den Port für die Kommunikation geöffnet. Die restlichen Ports sind zur Sicherheit blockiert.

#### Netzwerk

Die Server laufen in einer eigenen IP Range und somit in einem eigenen Netzwerk indem man nur mit fremden Geräten zugreifen kann, wenn man das Netzwerk kennt und wenn man einen der Accounts (oben beschrieben) mit Passwort kennt.

#### Starke Passwörter

Jeder Schutz ist wirkungslos, wenn man schnell hinter das Passwort vom root oder einem anderen Benutzer kommt. Deshalb wurde hier auf eine starke Passwort Wahl geschaut.



## K5

#### Vergleich Vorwissen - Wissenszuwachs

**Beginn vom Projekt:**

Vor dem Projekt wusste ich bereits was Docker war und für was es hauptsächlich zuständig war. Ich hatte jedoch noch keinerlei Erfahrung damit. Sprich wie man es bedient.

Vom theoretischem Aufbau von den Containern und dem Prizip habe ich auch bereits gehört. Jedoch fehlt mir auch hier die praktische Erfahrung.

**Nach dem Projekt:**

Mittlerweile habe ich eine menge dazugelernt was Docker angeht. Ich weiss nun was wirklich hinter der Technologie steckt. 

Ich verstehe nun warum es sinnvoll ist, eine Umgebung mit Docker aufzubauen und welche Vorteile es mit sich bring. 

Ich habe zusätzlich noch kurz Kubernetes kennen gelernt welches im Unterricht kurz angesprochen wurde. In der Theorie habe ich es zwar verstanden, jedoch nehme ich mir vor dies noch in der Praxis genauer anzuschauen.

Auch viel gelernt habe ich über die Microservices. Ich sehe viel potential in dieser Technologie was die Zukunft angeht und möchte auch weiterhin mehr darüber lernen um auf dem aktuellen Stand zu sein.

#### Reflexion

Ich habe viel gelernt und konnte meinen Horizont in diesem Thema erweitern. Dieses Thema ist sehr modern und es freut mich persönlich sehr so ein Modul absolvieren zu dürfen.
Ebenfalls habe ich Markdown kennengelernt, was mich persönlich als enthusiastischer Hobbyprogrammierer sehr freut.

Ich habe mein bestes bestes gegeben, so viel wie möglich mit zu nehmen.