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

    git merge BranchName
Mergt den angebenen Branch zu dem aktuellen Branch.
Dabei kann es zu einem Merge Conflict kommen:
    <<<<<<< HEAD
    Testtext
    =======
    Add some more text
    >>>>>>> develop
Hier sind die Inhalte welche auf beiden Branches gleichzeitig modifiziert wurden enthalten. Der Entwickler muss nun wählen was er davon behalten möchte.
Es wurde hier kein Merge-Request erzeugt, da dies ein spezielles Konstrukt von GitLab ist. Als Vorgehensweise bei Git bietet sich ein Merge und anschließender Pull-Request an.

    git branch
Verfügbare und aktuellen Branch anzeigen.

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

## Historie von Commits
    [lullrich@localhost GitExercise]$ git log
    commit cb4dd5f5c9b2111a1fa9441016876bfecc248bbf (HEAD -> master, origin/master, release)
    Merge: 6128dbc e413653
    Author: Lucas Ullrich <lullrich@localhost.localdomain>
    Date:   Mon Oct 19 20:22:25 2020 +0200

        PJM-0002 merge

    commit e4136539b871a07cf3c27b57103b9ab1a1b4846b (origin/develop, develop)
    Author: Lucas Ullrich <lullrich@localhost.localdomain>
    Date:   Mon Oct 19 20:19:35 2020 +0200

        PJM-0003 add some more sample text

    commit 6128dbc405a95d3aaf7fa7cc908b165d1e050b61
    Author: Lucas Ullrich <lullrich@localhost.localdomain>
    Date:   Mon Oct 19 20:17:23 2020 +0200

        PJM-0002 add sample text

    commit 59be1b0d46cb34247e0e2e1b19830ff60ccb6d6f
    Author: Lucas Ullrich <lullrich@localhost.localdomain>
    Date:   Mon Oct 19 20:17:03 2020 +0200

        PJM-0000 add Documentation

    commit f46aacefad45b5eceda12c5317921cc1ca7228bd
    Author: Lucas Ullrich <lullrich@localhost.localdomain>
    Date:   Mon Oct 19 20:03:54 2020 +0200

        PJM-0000 add Documentation

    commit 029fe39734decbc94f523c3562516cc1501aa0b8
    Author: Lucas Ullrich <lullrich@localhost.localdomain>
    Date:   Mon Oct 19 19:54:15 2020 +0200

        PJM-0000 Add Readme to project

    commit 932c2befd649d22c22c1136f3e2488d02a975b58
    Author: Lucas Ullrich <lullrich@localhost.localdomain>
    Date:   Mon Oct 19 18:55:51 2020 +0200

        PJM-0001 asdblnasd

Aufgrund des Ablaufs in der Anleitung wurden die zu beginn erstellten Branches verworfen (Git hat vorher einen remote benötigt).
Bei dem ältesten Commit handelt es sich um einen der Testcommits zum Implementieren des Hooks.

## Branches auf dem Repo
    [lullrich@localhost GitExercise]$ git branch
      develop
      feature/PJM-0001
    * master
      release

Das Repo findet sich unter:
https://github.com/LucasUllrich/PJMRepo

