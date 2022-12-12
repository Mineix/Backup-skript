# Backup-skript
Backup_skript

function backup(){
	BACKUPFILE=$(date +%Y%m%d-%H%M%S)-backup.tgz
	yesno-dialog compress COMPRESS "Sind Sie sicher das Sie die Option benutzen möchten?"
	yesno-dialog where2Backup WHERE2BACKUP "Sind Sie sicher das Sie hier in speichern möchten: "
	yesno-dialog what2Backup WHAT2BACKUP "Sind Sie sicher das Sie dieses Verzeichnis sichern wollen? "
	yesno-dialog compress 
	yesno-dialog where2Backup 
	yesno-dialog what2Backup 
	if [ ! -d ${WHERE2BACKUP} ]
        then
                echo "| Verzeichnis ${WHERE2BACKUP} erstellt."
                mkdir -p ${WHERE2BACKUP}
        fi
	tar cf${COMPRESS} ${WHERE2BACKUP}/$BACKUPFILE ${WHAT2BACKUP}
	sleep 2
}

function unbackup(){
	yesno-dialog where2Find WHERE2FIND "Wo sollen nach den Backups gesucht werden: "
	yesno-dialog where2Find
	for I in $(find ${WHERE2FIND} -maxdepth 1 \( -name "*tgz" -o -name "*xz" -o -name "*bzip2" \) )
	do 
	    echo "|> "$I
    done
	yesno-dialog where2Restore WHERE2RESTORE "Sind Sie sicher das Sie das BAckup hier entpacken wollen: "
    	done
	yesno-dialog what2Restore 
	yesno-dialog where2Restore
	if [ ! -d ${WHERE2RESTORE} ]
	do
	then
		echo "| Verzeichnis ${WHERE2RESTORE} erstellt."
		mkdir -p ${WHERE2RESTORE}
	done
	fi
	cd ${WHERE2RESTORE}
	tar xfz ${WHERE2FIND} ${WHERE2RESTORE}
	tar xfz ${WHAT2RESTORE} ${WHERE2RESTORE}
}

function listbackup(){
	yesno-dialog where2Find WHERE2FIND "Wo sollen nach den Backups gesucht werden: "
	yesno-dialog where2Find 
	for I in $(find ${WHERE2FIND} -maxdepth 1 \( -name "*tgz" -o -name "*xz" -o -name "*bzip2" \) )
	do 
	    echo "|> "$I
    done
	yesno-dialog what2List WHAT2LIST "Sind Sie sicher das Sie dieses Backup auflisten wollen?" 
	tar tf ${WHERE2LIST}
		echo "|> "$I
    	done
	yesno-dialog what2List 
	tar tvf ${WHAT2LIST}
	read -p "| Beliebige Taste drücken ..." 
	sleep 1
}

function deletebackup(){
	yesno-dialog where2Find WHERE2FIND "Wo sollen nach den Backups gesucht werden: "
	yesno-dialog where2Find 
	for I in $(find ${WHERE2FIND} -maxdepth 1 \( -name "*tgz" -o -name "*xz" -o -name "*bzip2" \) )
	do 
	    echo "|> "$I
    done
   	yesno-dialog what2Del WHAT2DEL "Sind Sie sicher das Sie dieses Backup löschen wollen?" 
	echo rm ${WHAT2DEL}
		echo "|> "$I
	done
   	yesno-dialog what2Del 
	rm ${WHAT2DEL}
	if [ $? = 0 ]
	then
		echo "| ${WHAT2DEL} wurde gelöscht!"
	fi
	sleep 2
}

43
funktionen.sh
@@ -5,6 +5,15 @@
# E-Mail : roesner@elektronikschule.de 
# Version: v02

# Variablen init
COMPRESS="z"
WHERE2BACKUP="/tmp"
WHAT2BACKUP="/home"
WHERE2FIND="/tmp"
WHERE2RESTORE="/tmp"
WHAT2LIST="/tmp"
WHAT2DEL="/tmp"


# Hauptmenu ausgeben
function menu(){
@@ -32,51 +41,67 @@ function compress(){
        echo "|      XZ:                    J                |"
        echo "|                                              |"
        read -p "| Eingabe: " COMPRESS
	echo "| Sind Sie sicher das Sie die Option \"${COMPRESS}\" benutzen möchten?"
}

# Eingeben des Backuppfades
function where2Backup(){
        clear
        #clear
        echo "|----------------------------------------------|"
        echo "| Wohin soll das Backup gespeichert werden:    |"
        echo "|                                              |"
        read -p "| Eingabe: " WHERE2BACKUP
	echo "| Sind Sie sicher das Sie hier \"${WHERE2BACKUP}\" in speichern möchten?"
}

# Eingeben des Pfades, der gesichert werden soll
function what2Backup(){
        clear
        #clear
        echo "|----------------------------------------------|"
        echo "| Welches Verzeichnis soll gesichert werden:   |"
        echo "|                                              |"
        read -p "| Eingabe: " WHAT2BACKUP
	echo "| Sind Sie sicher das Sie dieses Verzeichnis \"${WHAT2BACKUP}\" sichern wollen?"
}

# Eingeben des Pfades, wo gesucht werden soll nach Backups
function where2Find(){
	clear
	#clear
	echo "|----------------------------------------------|"
	echo "| Wo soll nach Backups gesucht werden:         |"
	echo "|                                              |"
	read -p "| Eingabe: " WHERE2FIND 
	echo "| Sicher, dass hier \"${WHERE2FIND}\" nach Backups gesucht werden soll? "
}

# Eingeben des Backuppfades
function where2Restore(){
        clear
        #clear
        echo "|----------------------------------------------|"
        echo "| Wohin soll das Backup ausgepackt werden:     |"
        echo "|                                              |"
        read -p "| Eingabe: " WHERE2RESTORE
	echo "| Sind Sie sicher das Sie das BAckup hier \"${WHERE2RESTORE}\" entpacken wollen?" 
}

# Eingeben des Backups, welches aufgelistet werden soll
function what2List(){
	clear
	#clear
	echo "|----------------------------------------------|"
	echo "| Welches Backups soll gezeigt werden:         |"
	echo "|                                              |"
	read -p "| Eingabe: " WHAT2LIST 
	read -p "| Eingabe: " WHAT2LIST
	echo "| Sind Sie sicher das Sie dieses Backup \"${WHAT2LIST}\" auflisten wollen?"
}

# Eingeben des Backups, welches aufgelistet werden soll
function what2Restore(){
        #clear
        echo "|----------------------------------------------|"
        echo "| Welches Backups soll gezeigt werden:         |"
        echo "|                                              |"
        read -p "| Eingabe: " WHAT2RESTORE
        echo "| Sind Sie sicher das Sie dieses Backup \"${WHAT2RESTORE}\" einspielen wollen?"
}

# Eingeben des BAckup im absoluten Pfad, welches gelöscht werden soll
@@ -85,15 +110,15 @@ function what2Del(){
	echo "| Welches Backups soll gelöscht werden:        |"
	echo "|                                              |"
	read -p "| Eingabe: " WHAT2DEL 
	echo "| Sind Sie sicher das Sie dieses Backup \"${WHAT2DEL}\" löschen wollen?"
}

function yesno-dialog(){
        YESNO=0
        until [ $YESNO = 1 ]
        do
                $1
                echo "| $3 \(${2}\)"  
                read -p "| (0: nein | 1: ja) " YESNO
		$1
		read -p "| (0: nein | 1: ja) " YESNO
        done
}


