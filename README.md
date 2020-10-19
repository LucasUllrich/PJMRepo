# PJMRepo
## Möglichkeiten zum Projektmanagement
Der Größte Vorteil, der sich mit Git bietet ist die Anpassbarkeit.
Git kann dabei sehr gut mit diversen Skripten erweitert werden, welche zu unterschiedlichen Zeiten aufgerufen werden und somit auch eine Integration in diverse Management Tools, vorrausgesetzt diese verfügen über ein CLI, angeschlossen werden.
So ist es beispielsweise möglich Arbeitspakete durch Stichworte wie "Progress" oder ähnlichem zu tracken und diesen Fortschritt auch in das PJM direkt mit dem erfolgten Commit einfließen zu lassen.

## Erstellen eines lokalen Repos
    git init
Erzeugt in dem aktuellen Pfad alle Abhängigkeiten für git. Hierfür wird der .git Ordner erstellt und enthält anschließend die notwendigen Konfigurationen.

    git checkout -b BranchName
Erzeugt einen entsprechend benannten Branch und wechselt auf diesen

    git add --all
Fügt alle Modifizierten Files in die Indexierung für den nächsten Commit hinzu.

    git commit -m "Message"
Erstellt einen Commit mit ensprechendem Text für die durch git add indizierten Files.

    git remote add origin URL
Fügt unter dem Namen origin einen Server hinzu auf den das Projekt gepusht werden kann.

    git push origin BranchName
Lädt die in den commits enthaltenen Daten auf den unter origin angegebenen Server mit entsprechendem Branch hoch.

## Beispielimplementierung commit hook
    #!/bin/bash

    # echo $0 $1
    # $0 is the commit-msg scripr
    # $1 is the file containing the commit message

    commit=$1
    RED='\033[0;31m'
    NC='\033[0m'

    # head reads -n [amount] numbers of lines of a file
    line=$(head -n 1 $commit)

    regex="^PJM-[0-9]{4}[^0-9]"

    if [[ $line =~ $regex ]]
    then
        exit 0
    else
        echo -e "${RED}ERROR - Commit missing issue number at beginning${NC}"
        exit 1
    fi

