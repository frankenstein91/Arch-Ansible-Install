On Hostsystem:

1. https://wiki.archlinux.org/index.php/archiso#Prepare_an_ISO_for_an_installation_via_SSH

2. mkdir work

3. sudo mkarchiso -v -w work -o ~/ISO-Downloads archlive

4. pikaur -S ansible

5. pikaur -S ansible-aur-git

6. Boot archiso on target system

7. ansible-playbook -i inventory.yml playbook.yml
