Esto me funcionó instalando el plutus 

## 1 - Install Nix

   [$] sh <(curl -L https://nixos.org/nix/install) --darwin-use-unencrypted-nix-store-volume

## 2 - Close terminal & reopen (to make sure that all environment variables are set)

*Ojo que el shell nunca me agarró inmediatamente el Nix así que hay que ejecutar el  /Users/<usuario>/.nix-profile/etc/profile.d/nix.sh*

## 3 - Check Nix installation / version with

   [$] nix --version
## 4 - Edit the /etc/nix/nix.conf file

   [$] nano /etc/nix/nix.conf
## 5 - Add these lines to the file:
~~~
substituters        = https://hydra.iohk.io https://iohk.cachix.org https://cache.nixos.org/
trusted-public-keys = hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=
~~~

Note: These lines are there to avoid very long build times

Note 2: if the file /etc/nix/nix.conf doesn't exist: create it. ([$] mkdir /etc/nix for the directory and [$] touch /etc/nix/nix.conf for the file)

Pueden chequear que el nix tenga los settings correctos con nix show-config

## 6 - Restart your computer

## 7 - Now to install, clone the git repo first

   [$] git clone https://github.com/input-output-hk/plutus.git
## 8 - All the following builds should be executed while in the plutus directory

   [$] cd plutus
## 9 - Build the Plutus Core (This may take some time :) be patient)

   [$] nix build -f default.nix plutus.haskell.packages.plutus-core.components.library
Note:

On MacOS BigSur some users have reported that the building failed with an error like:

error: while setting up the build environment: getting attributes of path '/usr/lib/libSystem.B.dylib': No such file or directory
To resolve this, we will change the nix build to an unstable (read: newer) build of nixpkgs.

   [$] sudo nix-channel --add https://nixos.org/channels/nixpkgs-unstable unstable
## 10 - Build the Plutus Playground Client / Server
~~~
[$] nix-build -A plutus-playground.client
[$] nix-build -A plutus-playground.server
~~~

## 11 - Build other plutus dependencies
~~~
[$] nix-build -A plutus-playground.generate-purescript
[$] nix-build -A plutus-playground.start-backend
[$] nix-build -A plutus-pab
~~~

## 12 - Go into nix-shell

   [$] nix-shell
## 13 - inside of the nix-shell
~~~

[$] cd plutus-pab
[$] plutus-pab-generate-purs
[$] cd ../plutus-playground-server
[$] plutus-playground-generate-purs
~~~

## 14 - start the playground server

   [$] plutus-playground-server


Great! All set.

___

## 15 - Now in a new terminal window:
~~~

[$] cd plutus
[$] nix-shell
[$] cd plutus-playground-client
~~~


~~~

>% source  /Users/mba/.nix-profile/etc/profile.d/nix.sh
>% nix-shell
>[nix-shell:~/Code/plutus]$ cd plutus-playground-client/
>[nix-shell:~/Code/plutus/plutus-playground-client]$ npm run start
~~~


## 16 - Here we compile / build the frontend of the playground

   [$] npm run start

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Mig29x/Cardano/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
