# C++

## Inhaltsverzeichnis

- [C++](#c)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
    - [OOP](#oop)
      - [Klasse definieren](#klasse-definieren)
      - [Virtal Void](#virtual-void)
    - [QT](#qt)
      - [Signals and Slots](#signals-and-slots)
    - [Sonstiges](#sonstiges)
      - [Ternitärer Operator](#ternitärer-operator)
      - [Reference](#reference)

### Code dokumentieren
https://developer.lsst.io/v/DM-5063/docs/cpp_docs.html

```cpp
/**
* @brief
*
*/
```

### OOP

#### Klasse definieren

Kann in Headerdatei ausgelagert werden, um anderen Files Klasse schnell bekannt zu machen:

```cpp
//myclass.h
#ifndef MYCLASS_H
#define MYCLASS_H

#include <superwichtigebib>
class myClass : public parentClass
{
    public:
    //Konstruktor
    myClass();
    //Destruktor
    ~myClass();
    //Methode
    int getVar1();
    void setVar1(int newvar1);

    private:
    int var1;
};
#endif
```

Weitere Definition dann in .cpp Datei:

```cpp
//myclass.cpp

#include "myclass.h" //Pfad zur Datei

int myClass::getVar1() {…
    return this->var1;
}

void myClass::setVar1(int newvar1) {
    this->var1 = newvar1;
}


```

####  Objekt erzeugen

* Auf Stack:

```cpp
MyClass myObject; //Standartkonstruktor
MyClass myObject (constructordata1, constructordata2); //eigener Konstruktor
```
* Auf Heap: (new gibt pointer zurück!)

```cpp
MyClass *pmyObject = new MyClass();
MyClass *pmyObject = new MyClass(data1, data2);
```
#### Vererbung

##### private vs. protected

private Elemente werden nicht mitvererbt, protected hingegen schon. Objekte können auf protected Elemente aller abgeleiteten Klassen zugreifen

##### Konstruktoren

Der Reihe nach wird aufgerufen:
→ Konstruktor Basisklasse 
→ Konstruktor abgeleitete Klasse

TODO

##### Methoden überschreiben

Methode in abgeleiteter Klasse muss gleiche Signatur haben. Überladene Methoden müssen auch in abgeleiteter Klasse überschrieben werden sonst überdeckt überschriebene Methode alle überladenen Methoden.

1. späte/dynamische Bindung (virtelle Funktion)

Funktion einer Basisklasse, die von erbender Klasse überschrieben werden kann. 

→ Virtual sorgt für späte Bindung

2. frühe Bindung/statische (während kompilieren)

```cpp
class BaseClass {
    // ...
    void whoami() {
        cout << "BaseClass";
    }
    virtual void virtualwhoami() {
        cout << "BaseClass";
    }
};

class DerivedClass : BaseClass
{
    void whoami() {
        cout << "DerivedClass";
    }
    void virtualwhoami() override { //override nicht zwingend, aber besser lesbar und Kompiler prüft
        cout << "DerivedClass";
    }
};

BaseClass *pbaseInstance = new BaseClass();
BaseClass *pderivedInstanceOnBasePointer = new DerivedClass();
DerivedClass *pderivedInstance = new DerivedClass();

//Statische Bindung
pbaseInstance->whoami(); // Output: "BaseClass"
pderivedInstance->BaseClass::whoami(); //Output "BaseClass"
pderivedInstance->whoami(); // Outpur: "DerivedClass"

//Dynamische Bidnung, abhängig von Klasse wird entsprechende Methode aufgerufen, trotz gleichem Pointer
pbaseInstance->virtualwhoami(); //Output "BaseClass"
pderivedInstanceOnBasePointer->virtualwhoami(); //Output "derivedClass"
```

#### Pure Virtalfunctions

→ müssen überschrieben werden, sonst ist erbende Klasse auch Abstrakt
→ kann Body haben, macht aber keinen Sinn

```cpp
virtual void virtualMethod();
virtual void pureVirtualMethod() = 0;
```

### QT

#### Signals and Slots

Slots: Methoden von QObject-Klasse (Klasse muss von QObject erben). Werden durch Signale aufgerufen (vgl. Callbackfunktionen)

Connect: (nur Übergabetyp muss angegeben werden, kein Variablenname), Arten nur vor Multithreading relevant
* Qt::DirectConnection : Slot wird auch bei Mulitthreading direkt ausgeführt, Slot muss Threadsafe sein !!
* Qt::QueuedConnection : Sheduler entscheidet wann Slot ausgeführt wird
* wenn nicht angegeben, entscheidet Qt was am besten ist
```cpp
//myClass.h

#ifndef MYCLASS_H
#define MYCLASS_H

#include <QObject>
class myClass : public QObject
{
    Q_OBJECT
    public: //...
    private: //...
    signals:
        void sendData(*char Data);
        //Braucht keine weitere Spezifikation
    public slots:
        void processData();
}
#endif
```

```cpp
#include "myclass.h" //Pfad zur Datei

myClass::myClass() {
    connect(*sender, SIGNAL(signalname(type), *receiver, SLOT(slotname(type)));
}

void myClass::processData() {
    //....
    emit sendData(*pData);
}
```

Signal: Löst Slot aus

```cpp
//an Stelle wo Signal gesendet werden soll:
emit signal();
```

### Sonstiges

#### Reference

Wie Alias für Variablen/Objekte.
Kein Pointer, deshalb gleicher Typ wie referenzierter Wert

Refernces vs Pointers:
* kein Nullpointer für Referenz
* Referenz kann nicht geändert werden
* Refernz muss bei Erstellung initialisiert werden

```cpp
myobj a = new myobj();
myobj &r = a; //Referenz auf a --> Tatsächliche Variable wird benutzt
//Funktionsdeklaration
func(int &a) {
a.dosth();
}
//Funktionsaufruf
func(a);
```

#### Ternitärer Operator

```cpp
variable = (expression-1) ? expression-2 : expression-3

if (Expression1)
{
    variable = Expression2;
}
else
{
    variable = Expression3;
}
```

#### Enum

Wie alias für Ints
```cpp
enum colors {red, green, blue}; //red=0 green=1 blue=2
enum colors {red=1, green, blue}; //Compiler weißt green und blue Werte zu

enum colors s = red; //s=1
```
# Vererbung

## Konstruktor

```cpp
class Employee {
    Employee(values);
    Employee()
}
    
class Developer : public Employee // mit public bleiben public methods und member public, sonst private
{
    Developer(values, devvalue) : Employee(values), devValue(devvalue) // ruft Employee(values) auf
    Developer(devvalue) : mdevalue(devvalue) // ruft Employee() auf 
}
```

## g++
### Compilieren:
```bash
g++ -c datei1.cpp
```
Symbole aus .o-Datei anzeigen, Linkvorgang zu Debuggen (z.B. multiple Defintion):
```bash
nm   datei1.o
# definierte Namen anzeigen statt C-Compilernamen 
nm -C datei.o
```
U = undefiniend (fehlt)
T = enthält Binarys für dieses Symbol
### Linker
Verschiedene .o-Dateien zusammen linken:
```bash
g++ datei1.o datei2.o
```
Bibliotheken linken:
```bash
g++ -l superlib 
```
## Bibliotheken

Haben zu Beginn einen Index, der auf entsprechende Stelle für entsprechende Funktion verweist
.a = archive *statische* Bib
Code wir mitcompiliert

`g++ -static Test.o -lgtest`

sucht nach .a- Dateien für gtest

.so = shared object *dynamische* Bib

Mit bereits kompilierten Binaries wir gelinkt, default für g++

--> Kompilierte Bib muss auf Hostsystem vorhanden sein

### Bibliothek bauen

- Statisch:
		`ar cr libNAME.a Datei1.0 Datei2.o`
- Dynamisch: `g++ -fpic -shared -o libNAME-so Datei1.o`
### Suchpfade setzen

`ldd` zeigt gelinkte Bibliotheken für Binary an

`g++ -L PFAD` gibt spezifischen Suchpfad für jeweiligen Kompiliervorgang an

Pfade in `/etc/ld.so.conf.d` hinzufügen, `ldconfig `ausführen