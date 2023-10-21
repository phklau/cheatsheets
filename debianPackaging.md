# Debian Pakete erstellen
Referenz: https://www.linuxshelltips.com/create-debian-package/
## Inhalt

- [Paketaufbau](i)
- [Vorrausetzungen](#voraussetzungen)

## Voraussetzungen

* build-essentials
* devscripts
* debhelper
* dpkg-dev

## Paketaufbau

1. upstream tarball (enthält die Software, die gepakt werden soll)
2. Sourcepackage(config für Build, Dependencys) .dsc
3. Binarypackage(executables, configs, ...) .deb --> wird aus Sourcepackage gebaut

## Ordnerstruktur des ungepackten Pakets
```
halloPackage
├── DEBIAN
│   └── control
└── usr
    └── bin
        └── hallo
```


## Workflow (Compilierte Anwendung)

1. exectuatle builden mit Compiler
2. neuen Ordner für Paket
    - in diesem Ordner DEBIAN Ordner mit folgenden Files erstellen:
       - control(Pflicht)
```
        #Paketname
        Package: debpackage
        #Version
        Version: 0.1
        #Paketabhängigkeiten
        Depends: 
        # armhf für ARM
        Architecture: amd64
        Maintainer: Hans Dieter <greenhorn@debian.org>
        Description: Greatest Program the World has seen!
```
- changelog
- ...

3. Installationspfad als eigene Verzeichnisstruktur im Paketordner erstellen (hier wird später das Programm installiert)
```bash
        #z.B.
        mkdir -p paketordner/usr/bin
```
5. Executable in Installationspfad verschieben
6. Packen
```bash
   dpkg-deb --build paketordner zielordner
```
7. Installieren
```bash
    sudo dpkg -i PACKET.deb
```
