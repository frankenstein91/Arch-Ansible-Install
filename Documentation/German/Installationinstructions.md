# Installationsanleitung

Es stehen zwei Wege zur Installation bereit. Empfohlen wird der erste sichere und immer aktuelle Weg zur Installation, dieser Weg benötigt ein bereits installiertes ArchLinux System auf einem beliebigen Computer oder VM mit Zugang zum Netzwerk. Außerdem wird für beide Wege ein Netzwerk benötigt, indem alle Geräte die installiert werden sollen erreichbar sind, dazu zählt auch ein DHCP und eine Internetverbindung.

## manuelle Vorbereitung und Installation

Dies ist der empfohlene Weg zur Durchführung der ArchLinux Installation mit diesem Skript. Ein einfacherer, aber weniger sicherer und weniger aktueller Weg wird weiter unten beschrieben.

Für diesen Installationsweg benötigt man ein bereits installiertes System mit ArchLinux und einen Internetzugang. Als erster Schritt wird für die Netzwerkinstallation ein eigenes CD-Abbild erstellt. Dafür bietet das Arch-Wiki eine gute [Anleitung](https://wiki.archlinux.org/index.php/archiso#Prepare_an_ISO_for_an_installation_via_SSH). Da diese Anleitung aber nicht ganz vollständig ist und damit nicht einsteigerfreundlich, bietet sich die einfache Schrittanleitung an.

Diese Schritte werden alle auf dem eigenen ArchLinux-System ausgeführt und (bis auf eine Ausnahme) nicht auf den zu installierenden Systemen.

1. `sudo pacman -Sy archiso`

2. `mkdir Linux4school && cd Linux4school`

3. `cp -r /usr/share/archiso/configs/releng archlive`

4. `mkdir -p archlive/airootfs/etc/systemd/system/multi-user.target.wants`

5. `ln -s /usr/lib/systemd/system/sshd.service archlive/airootfs/etc/systemd/system/multi-user.target.wants/`
   *bei diesem Befehl kann der Fehler `ln -s /usr/lib/systemd/system/sshd.service archlive/airootfs/etc/systemd/system/multi-user.target.wants/` auftreten, er kann ignoriert werden.*

6. `mkdir archlive/airootfs/root/.ssh`

7. `ssh-keygen -t ed25519 -f ./masterKey -N ""`

8. `cat masterKey.pub >> archlive/airootfs/root/.ssh/authorized_keys`

9. öffne die Datei **archlive/profiledef.sh** in einem Editor der Wahl

   1. finde in der Datei den Abschnitt "file_permissions". Aktuell müsste er ungefähr so aussehen:
      `file_permissions=(
        ["/etc/shadow"]="0:0:400"
        ["/root"]="0:0:750"
        ["/root/.automated_script.sh"]="0:0:755"
        ["/usr/local/bin/choose-mirror"]="0:0:755"
        ["/usr/local/bin/Installation_guide"]="0:0:755"
        ["/usr/local/bin/livecd-sound"]="0:0:755"
      )`

   2. füge folgende zusätliche Zeilen in die Liste zwischen den runden Klammern ein
      `  ["/root/.ssh"]="0:0:0700"
        ["/root/.ssh/authorized_keys"]="0:0:0600"`

   3. speichere und schließe die Datei

10. `mkdir work`

11. `sudo mkarchiso -v -w work -o ./ archlive`

12. `git clone https://github.com/actionless/pikaur.git`

13. `git clone https://github.com/frankenstein91/Arch-Ansible-Install.git`

14. `cd pikaur`

15. `python3 ./pikaur.py -S ansible ansible-aur-git`

16. `cd ..`

17. Nun ist der Schritt gekommen, an dem wir das erstellte ISO auf eine CD brennen oder mit [balenaEtcher](https://www.balena.io/etcher/) auf einen USB-Stick übertragen müssen. *Dieser Schritt muss wiederholt werden, wenn mehrere Systeme parallel installiert werden sollen.*

18. die Systeme von den tragbaren Medien booten, dabei die Booteinstellungen im UEFI auf der Festplatte als erstes Medium belassen

19. per `ip a s` die IP-Adressen der Systeme bestimmen und am besten notieren

20. die Datei **Arch-Ansible-Install/inventory.yml** in einem Editor der Wahl öffnen

    * diese Datei ist im Yaml-Format, Einrückungen sind <mark>wichtig</mark>

    * unter dem Ast "hosts:" muss für jedes System ein Eintrag mit eindeutigem Namen erstellt werden. Im Beispiel nutzen wir dafür `archiso`. Dieser Name wird nur in der Config genutzt und hat keine weitere Bedeutung.

    * der `ansible_user` ist immer `root`

    * die Information `ansible_host` wird jeweils mit der notierten IP gefüllt

21. `cd Arch-Ansible-Install/`

22. `ansible-playbook -i inventory.yml playbook.yml`

Ab hier installiert das Hauptsystem alle genannten Systeme

## einfache Installation mit VM

<mark>ToDo</mark>: *VM und ISO erstellen*

<mark>ToDo</mark>: *Anleitung zur Nutzung schreiben*

<mark>ToDo</mark>: *VM und ISO bereitstellen*
