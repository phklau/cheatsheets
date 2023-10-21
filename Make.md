# Make
## Makefile
### Aufbau
```cpp
<TARGET>: <Prerequisites>
<TAB><Instructions>
```
### Aufruf
```bash
make <Target>
```
- Prerequisites werden bei Aufruf eines Targets automatisch der Reihe nach ausgeführt
- bei Aufruf von make wird automatisch erstes Target ausgeführt
- auf Tabs und Leerzeichen achten
### Beispiel
```cpp
CC = g++
all: compile:
	./Main.o
compile:
	$(CC) -o Main.o Main.cpp
```

### File-Targets

```bash
Main: foo.o main.o
	g++ -o Main foo.o main.o

foo.o: foo.cpp foo.h
	g++ -c foo.cpp

main.o: main.cpp
	g++ -c main.cpp
```

Make führt Target incl. Dependencys aus wenn:
- Target-Datei nicht existiert 
- Dependencys neuer als Target sind
