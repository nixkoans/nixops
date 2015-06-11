# Install NixOps on Mac OS X

Where's our nixops utility?

```
$ nix-env -qaP nixops
nixpkgs.nixops          nixops-1.2
nixpkgs.nixopsUnstable  nixops-1.3pre1486_7489764
```

Cool. Let's use the stable nixops 1.2 version.

```
$ nix-env -iA nixpkgs.nixops
```

# What can we do with NixOps?

```
$ nixops -h
usage: nixops [-h] [--version]
              {list,create,modify,clone,delete,info,check,set-args,deploy,send-keys,destroy,stop,start,reboot,show-physical,ssh,ssh-for-each,scp,rename,backup,backup-status,remove-backup,clean-backups,restore,show-option,list-generations,rollback,delete-generation,show-console-output,dump-nix-paths,export,import,edit}
              ...

NixOS cloud deployment tool

positional arguments:
  {list,create,modify,clone,delete,info,check,set-args,deploy,send-keys,destroy,stop,start,reboot,show-physical,ssh,ssh-for-each,scp,rename,backup,backup-status,remove-backup,clean-backups,restore,show-option,list-generations,rollback,delete-generation,show-console-output,dump-nix-paths,export,import,edit}
                        sub-command help
    list                list all known deployments
    create              create a new deployment
    modify              modify an existing deployment
    clone               clone an existing deployment
    delete              delete a deployment
    info                show the state of the deployment
    check               check the state of the machines in the network
    set-args            persistently set arguments to the deployment
                        specification
    deploy              deploy the network configuration
    send-keys           send encryption keys
    destroy             destroy all resources in the specified deployment
    stop                stop all virtual machines in the network
    start               start all virtual machines in the network
    reboot              reboot all virtual machines in the network
    show-physical       print the physical network expression
    ssh                 login on the specified machine via SSH
    ssh-for-each        execute a command on each machine via SSH
    scp                 copy files to or from the specified machine via scp
    rename              rename machine in network
    backup              make snapshots of persistent disks in network
                        (currently EC2-only)
    backup-status       get status of backups
    remove-backup       remove a given backup
    clean-backups       remove old backups
    restore             restore machines based on snapshots of persistent
                        disks in network (currently EC2-only)
    show-option         print the value of a configuration option
    list-generations    list previous configurations to which you can roll
                        back
    rollback            roll back to a previous configuration
    delete-generation   remove a previous configuration
    show-console-output
                        print the machine's console output on stdout
    dump-nix-paths      dump Nix paths referenced in deployments
    export              export the state of a deployment
    import              import deployments into the state file
    edit                open the deployment specification in $EDITOR

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
```
