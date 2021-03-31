On Hostsystem:
  https://wiki.archlinux.org/index.php/archiso#Prepare_an_ISO_for_an_installation_via_SSH
  mkdir work
  sudo mkarchiso -v -w work -o ~/ISO-Downloads archlive
  pikaur -S ansible
  pikaur -S ansible-aur-git
