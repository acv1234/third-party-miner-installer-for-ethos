# third-party-miner-installer-for-ethos
This is an introduction to installing third party miners to ethOS with instructions and examples for miner developers to integrate and make ethOS installers / update scripts.

The instructions here are not supported by ethOS. This will install the files for one miner and use with another miner will overwrite previous installs.

ethOS installs its miners and associated files to /opt/miners/MinerFolder/MinerFiles

ethOS has six scripts to manage miners, they are all in /opt/ethos/
- bin/hash-monitor
  - "function get_hashrates()" is responsable for setting the method to connect to the miners api, pull the hash rate, and making it readable
    - add an IF miner condition with a json_decoded raw variable
    - add an IF miner condition to clean the output for each gpu from the raw json input
- lib/minerprocess.php
  - This file sets all flags the miner uses when it starts, set any and all conditions it needs and then build the start command. 
    - add IF miner condition to function start_miner()
      - If ethOS has a config setting that would apply to the miner this is where it is set
    - add "$miner_path"
    - add "$miner_params"
    - add "$miner_suffix"
- lib/functions.php
  - This file sets where to get hash 
- bin/minestop (ez)
  - This file stops the miner
    - add miner name to the "ps uax" line.
- etc/screenrc.miner (ez)
  - This file holds the parameters for the screen session
    - replace an example with miner name
- sbin/ethos-miner-monitor (ez)
  - This file grabs the miner version and sets autoreboot conditions
    - append miner name and version to "miner.versions"
- sbin/ethos-readconf (ez)
  - This file shows the user what miner is valid for their gpus and allows ethos to choose that miner
    - add miner name to "f.validminers", has 1 instance for each kind of gpu the miner works with
    - add miner name to "case "$MINER" in" has 1 instance of the miner to choose the method of pulling the process ID

This Git repository has branches to help with the installation, and reversion of a miner as well as show changes between them, IE https://github.com/cynixx3/third-party-miner-installer-for-ethos/compare/ethos-1.3.3...phoenixminer

With the understanding that ethOS users are either very new to linux or are mining farms with many rigs to install to, this repo will suggest using a method to install with a single command rather than the more linux approved method of have them do it. 

`source <(curl -s https://raw.githubusercontent.com/MinerAuthor/MinerName/master/miner-installer-for-ethos) install`

Or suggest updating with 

`source <(curl -s https://raw.githubusercontent.com/MinerAuthor/MinerName/master/miner-installer-for-ethos) update`

Or uninstall with

`source <(curl -s https://raw.githubusercontent.com/MinerAuthor/MinerName/master/miner-installer-for-ethos) revert`
