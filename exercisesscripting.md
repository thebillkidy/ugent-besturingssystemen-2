# Scripting oefeningen
## Tips & Tricks
### Check of commando successvol
```bash
if !find ... ; then
...
else
...
fi
```

### Geen dubbele haakjes bij commando check
```bash
if [[ echo ... ]]; then

fi
```

In bovenstaande if statement hebben we [[]] haakjes, echter zijn deze niet nodig omdat we kijken naar de exit code. Daarvoor is volgend voldoende:

```bash
if echo ... ; then
fi
```

## # 89
### Vraag
Ontwikkel een shellscript dat als (enige) parameter een bestandsnaam heeft. Als output moet het script het gemiddelde aantal karakters per regel en het gemiddelde aantal woorden per regel uitschrijven. Gebruik de output van het commando `wc` en probeer met de diverse methodes die hierboven aangehaald werden (behalve de tweede) om deze output op te splitsen.
Opgelet: de shell behandelt alle getallen als
integers. Niettemin kun je er, met behulp van wat wiskunde, voor zorgen dat delingen correct worden afgerond. Hoe ?

### Oplossing
```bash
#!/bin/bash
regel=0
words=0
chars=0
while read line
do
        word_count=$(echo $line | wc -w)
        char_count=$(echo $line | wc -m)

        regel=$((regel+1))
        words=$((words += word_count))
        chars=$((chars += char_count))
done <$1

echo "AVG Words: "$((words/regel))
echo "AVG Chars: "$((chars/regel))
```

## # 90
### Vraag
Wat moet je doen opdat de read - instructie lijnen niet alleen in woorden zou opsplitsen op basis van spaties, tabs en regeleinden, maar ook op basis van :- tekens?

### Oplossing
```bash

```

## # 91
### Vraag
Ontwikkel een script om de gebruikersnaam (veld 1 van /etc/passwd) te bepalen aan de hand van een gebruikers-
ID (veld 3 van /etc/passwd) dat je als (enige) parameter aan het script meegegeven hebt. Je kunt de output van het commando grep zowel met cut, read, set als met een array analyseren.

### Oplossing
```bash
# Oplossing met cut
grep -E '^.*:.*:172:.*$' /etc/passwd | cut -d ':' -f 1

# Oplossing met read
grep -E '^.*:.*:172:.*$' /etc/passwd | { IFS=: ; read naam rest; echo $naam; }
```

## # 92
### Vraag
Ontwikkel een script dat het commando tail n simuleert. Als eerste argument moet een bestandsnaam opgegeven worden en als tweede argument mag het aantal regels
opgegeven worden. Ontbreekt het tweede argument, dan worden de 10 laatste regels weergegeven. Realiseer dit op twee manieren:

* Gebruik een while-lus met een read-commando om het bestand te overlopen en een array om de gegevens cyclisch op te slaan.
* Gebruik geen array, maar bepaal vooraf het aantal lijnen van het bestand. 


### Oplossing
#### While-lus met read-commando
```bash
#!/bin/bash
# Wees zeker dat n opgegeven, anders zet op 10
n=$2

if [[ -z "$n" ]]; then
        n=10
fi

# Declare the array
declare -a ARRAY

# Read lines into the array
i=0
while read line
do
        ARRAY[i]=$line
        i=$((i+1))
done < $1

# Print n lines
j=$((i-n))
for j in $(seq 1 $n)
do
        echo ${ARRAY[j]}
doen
```

#### Vooraf bepaalde aantal lijnen
```bash
#!/bin/bash
# Wees zeker dat n opgegeven, anders zet op 10
n=$2

if [[ -z "$n" ]]; then
        n=10
fi

LINE_COUNT=`wc -l < $1`
START_PRINT=$(($LINE_COUNT - $n))

# Read lines and print as soon as i = START_PRINT
i=0
while read line
do
        if [[ $i -ge $START_PRINT ]]; then
                echo $line
        fi
        i=$(($i+1))
done < $1
```

## # 93
### Vraag
Hoe kun je met behulp van de while-of until-lus een aantal commando's oneindig lang laten uitvoeren? Onderbreek de uitvoering met Ctrl+C

### Oplossing
```bash
while (true)
do

done
```

## # 94
### Vraag
Het bestand ping.out bevat de output van een Windows batch file : ping -n 1 AL005951 ping -n 1 AL005952 ...
Indien toestel xxxxxxxx actief is, wordt het commando
ping beantwoord met regels van de vorm:

    Pinging xxxxxxxx [141.96.126.137] with 32 bytes of data: 
    Reply from 141.96.126.137: bytes=32 time=1322ms TTL=124

Je kunt niet-actieve toestellen herkennen aan het feit dat het commando niet wordt beantwoord zoals bij actieve. De foutboodschappen die dan gegenereerd worden zijn divers. Maak een Bash-script dat uit ping.out een inventaris opmaakt van alle niet- actieve toestellen (één lijn per toestel). Het script moet ook een samenvattende regel weergeven die zowel het aantal actieve als het totaal niet-aantal toestellen vermeldt.
Zorg ervoor dat het script onafhankelijk is van de precieze foutboodschappen die niet-actieve toestellen produceren. Construeer twee oplossingen, al dan niet gebruikmakend van associatieve arrays (enkel beschikbaar in Bash v4). 

### Oplossing
```bash

```