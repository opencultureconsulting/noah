# Datenintegration für noah.nrw
Harvesting von OAI-PMH-Schnittstellen und Transformation in METS/MODS für das Portal [noah.nrw](https://noah.nrw/)

**:warning: Dies ist ein Prototyp für die Beta-Version des Portals.**

## Datenfluss

![Datenflussdiagramm](flowchart.svg)

## Verwendete Tools

* Harvesting (mit Cache): [metha](https://github.com/miku/metha/)
* Transformation: [OpenRefine](https://github.com/OpenRefine/OpenRefine) und [openrefine-client](https://github.com/opencultureconsulting/openrefine-client)
  * :warning: Für den Produktivbetrieb ist der Einsatz von [metafacture](https://github.com/metafacture) geplant.
* Task Runner: [Task](https://github.com/go-task/task)

## Systemvoraussetzungen

* GNU/Linux (getestet mit Fedora 32)
* JAVA 8+
* [cURL](https://curl.se), xmllint

## Installation

1. Git Repository klonen

    ```sh
    git clone https://github.com/opencultureconsulting/noah.git
    cd noah
    ```

2. [metha 0.2.20](https://github.com/miku/metha/releases/tag/v0.2.20)

    a) RPM-basiert (Fedora, CentOS, SLES, etc.)

    ```sh
    wget https://github.com/miku/metha/releases/download/v0.2.20/metha-0.2.20-0.x86_64.rpm
    sudo dnf install ./metha-0.2.20-0.x86_64.rpm && rm metha-0.2.20-0.x86_64.rpm
    ```

    b) DEB-basiert (Debian, Ubuntu etc.)

    ```sh
    wget https://github.com/miku/metha/releases/download/v0.2.20/metha_0.2.20_amd64.deb
    sudo apt install ./metha_0.2.20_amd64.deb && rm metha_0.2.20_amd64.deb
    ```

3. [Task 3.2.2](https://github.com/go-task/task/releases/tag/v3.2.2)

    a) RPM-basiert (Fedora, CentOS, SLES, etc.)

    ```sh
    wget https://github.com/go-task/task/releases/download/v3.2.2/task_linux_amd64.rpm
    sudo dnf install ./task_linux_amd64.rpm && rm task_linux_amd64.rpm
    ```

    b) DEB-basiert (Debian, Ubuntu etc.)

    ```sh
    wget https://github.com/go-task/task/releases/download/v3.2.2/task_linux_amd64.deb
    sudo apt install ./task_linux_amd64.deb && rm task_linux_amd64.deb
    ```

4. Install task ausführen, um [OpenRefine 3.4.1](https://github.com/OpenRefine/OpenRefine/releases/tag/3.4.1) und [openrefine-client 0.3.10](https://github.com/opencultureconsulting/openrefine-client/releases/tag/v0.3.10) herunterzuladen

   ```sh
   task install
   ```


## Nutzung

* Vorab ggf. ulimit erhöhen, um Abbruch durch "too many open files" zu vermeiden

    ```
    ulimit -n 20000
    ```

* Alle Datenquellen (parallelisiert)

    ```
    task
    ```

* Eine Datenquelle

    ```
    task siegen:main
    ```

* Zwei Datenquellen (parallelisiert)

    ```
    task --parallel siegen:main wuppertal:main
    ```

* Trotzdem Verarbeitung starten, auch wenn Checksummenprüfung ergibt, dass nichts zu tun wäre

    ```sh
    task siegen:main --force
    ```

* Zur Fehlerbehebung: Befehle ausgeben, aber nicht ausführen

    ```sh
    task siegen:main --dry --verbose --force
    ```


* Links einer Datenquelle überprüfen

    ```
    task siegen:linkcheck
    ```

* Cache einer Datenquelle löschen

    ```
    task siegen:delete
    ```

* Verfügbare Tasks auflisten

    ```
    task --list
    ```

## Konfiguration

* Der Workflow einer Datenquelle wird im jeweiligen spezifischen `Taskfile.yml` definiert
  * Beispiel: [siegen/Taskfile.yml](siegen/Taskfile.yml)
* Die im Workflow verwendeten OpenRefine-Transformationsregeln liegen im Unterordner `config` der jeweiligen Datenquelle
  * Beispiel: [siegen/config/hbz.json](siegen/config/hbz.json)
* Allgemeine Tasks (z.B. Validierung) werden im [Taskfile.yml](Taskfile.yml) des Hauptordners definiert.

## OAI-PMH Data Provider

Für die Bereitstellung der transformierten Daten wird der dateibasierte OAI-PMH Data Provider [oai_pmh](https://github.com/opencultureconsulting/oai_pmh) genutzt. Installations- und Nutzungshinweise sind dort zu finden.
