# Sync methods
emaint -a sync				// Sync all repositories that are set to auto-sync including the Gentoo ebuild repository
emerge-webrsync				// Sync the Gentoo ebuild repository using the mirrors by obtaining a snapshot that is (at most) a day old
eix-sync					// emerge --sync now runs the emaint sync module with the --auto option
qlist -IRv				// List installed packages with version number and name of overlay used
eix --world | less			// To view the list of packages in the world set, along with their available versions, it is possible to use
eix --color -c --world | less -R		// To keep color in the output, use the --color switch

# Package installation (man emerge)
emerge -pv www-client/firefox		// List what packages would be installed, without installing them
emerge -av www-client/firefox		// List what packages would be installed, ask for confirmation before installing them
emerge -a =www-client/firefox-96.0.1	// Install a specific version of a package temporarily, to make it persistent, add "<www-client/firefox-96.0.1" to /etc/portage/package.mask
emerge -a1 www-client/firefox		// Install a package without adding it to the world file (--oneshot, -1)

# Package removal
emerge -W www-client/firefox		// Remove atoms and/or sets from the world file (--deselect [ y | n ], -W)
emerge -pc				// ^Review to make sure no required packages would be removed
emerge -ac				// ^Once it has been assured that emerge -c will only remove unneeded packages, remove packages
emerge -avc www-client/firefox		// To directly remove a package that no other packages depend on. (--depclean, -c) wont remove packages unless everything has been resolved. Sometimes it's necessary to update first (as below).
emerge -avuND @world			// Upgrade all packages in the world and their dependencies (--deep, -D)
emerge -avuUD @world			// ^Use (--changed-use, -U) in place of (--newuse, -N) to avoid rebuilds
emerge -aquUD @world			// ^Use the (--quiet, -q) flag for more succinct execution

# Package troubleshooting
revdep-rebuild -v			// Check for and rebuild missing libraries (not normally needed):
equery b `which vim`			// Tell which installed package provides a command using equery (app-portage/gentoolkit)
e-file vim				// Tell which (not) installed package provides a command using efile (app-portage/pfl)
equery d www-client/firefox		// Tell which packages depend on a specific package using equery
eix www-client/firefox			// Get information about a package using eix
emerge @module-rebuild			// After installing a new kernel
emerge @golang-rebuild			// After installing new version of go
emerge @preserved-rebuild		// For using newer libraries

# Portage enhancements
dispatch-conf				// Manage configuration changes after an emerge completes:

# After installations or updates
perl-cleaner --all			// Or if previous didn't help: perl-cleaner --reallyall -- -av
haskell-updater				// For haskell packages

# USE flags (man euse)
euse -i X				// Obtain descriptions and usage of the USE flag X using euse
equery hasuse mysql			// Show what packages have the mysql USE flag
eix --installed-with-use mysql		// Show what packages are currently built with the mysql USE flag:
equery uses <package-name>		// Show what USE flags are available for a specific package:

# Log management (man genlop)
emerge -a app-portage/genlop		// Install app-portage/genlop by issuing
genlop -l | tail -n 10			// View the last 10 emerges (installs)
genlop -t libreoffice			// View how long emerging LibreOffice took
emerge -pU @world | genlop --pretend	// Estimate how long emerge -uND --with-bdeps=y @world will take
watch genlop -unc			// Watch the latest merging ebuild during system upgrades:

# Overlays
eselect repository list			// List all existing overlays (app-eselect/eselect-repository)
eselect repository list -i		// List all installed overlays

# OpenRC services
rc-update add sshd default		// Start the ssh daemon in the default runlevel at boot
rc-service sshd start			// Start the sshd service now
rc-service sshd status			// Check if the sshd service is running
rc-service sshd restart			// Restart the sshd service
rc-service sshd stop			// Stop the sshd service
rc-status --all				// List services, their status, and the runlevels they belong to
rc-update show				// Show enabled services and the runlevels they belong to

# Systemd services
systemctl enable sshd			// Start the ssh daemon at boot:
systemctl start sshd			// Start the sshd service now
systemctl status sshd			// Check if the sshd service is running

# Tips
emerge -s "%^python$			// To search packages in Portage by regular expressions
emerge --regen				// Generate local metadata cache after syncing overlays
qcheck vim-core				// Use qcheck to verify installed packages (app-portage/portage-utils)
