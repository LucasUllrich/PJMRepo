# PJMRepo
## Möglichkeiten zum Projektmanagement
Der Größte Vorteil, der sich mit Git bietet ist die Anpassbarkeit.
Git kann dabei sehr gut mit diversen Skripten erweitert werden, welche zu unterschiedlichen Zeiten aufgerufen werden und somit auch eine Integration in diverse Management Tools, vorrausgesetzt diese verfügen über ein CLI, angeschlossen werden.
So ist es beispielsweise möglich Arbeitspakete durch Stichworte wie "Progress" oder ähnlichem zu tracken und diesen Fortschritt auch in das PJM direkt mit dem erfolgten Commit einfließen zu lassen.

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
