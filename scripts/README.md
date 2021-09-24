# admin scripts

when setting up a new machine for hackathons:

```sh
# run from the top of the source tree
$ scripts/create_hackathon_account
```

after that, the flow of the admin steps is:

## foreach hackathon:

1. create a local clone of the hackathon repo in `/home/hackathon`, e.g.:

```sh
$ cd /home/hackathon
$ git clone https://githib.com/pmemhackathon/2019-07-07
```

2. create_pmemusers (creates all 200 accounts), e.g.:

```sh
$ create_pmemusers 2019-07-07
```

## foreach session with same users (or on system boot)

1. start the webhackathon daemon as root

```sh
$ cd /home/hackathon
$ ./webhackathon reponame &
$ enable_pmemusers 1 100 todayspasswd
```

...hack...hack...hack

if system is rebooted, restart containers:

```sh
$ docker start $(docker ps -aq -f name=pmemuser)
```

users can be disabled selectively using docker stop.

## after everyone is done and won't come back:

1. kill the webhackathon daemon
2. delete_pmemusers (removes all pmemuserX accounts)

**Note**: we assume ID:GID 5000:5000 for the hackathon account and the range 5001:5001 through 5200:5200 are available for the 200 user accounts created.  edit all the scripts and change $BASEID if that has to change for some reason.
