Sample of Serf + Docker
====

## Build image

```bash
$ docker build -t tcnksm/serf .
```

## Create cluster

Run first agent

```bash
$ docker run -d -t --name serf1 -p 7979 tcnksm/serf serf agent -bind 0.0.0.0:7979
```

### Join cluster from other container (Use link)

```bash
$ docker run -d -t --name serf2 -p 7979 --link serf1:serf tcnksm/serf /bin/bash -c 'serf agent -bind 0.0.0.0:7979 -join $SERF_PORT_7979_TCP_ADDR:7979'
$ docker run -d -t --name serf3 -p 7979 --link serf1:serf tcnksm/serf /bin/bash -c 'serf agent -bind 0.0.0.0:7979 -join $SERF_PORT_7979_TCP_ADDR:7979'
```

### Join cluster from other container (Use IP inspect)

You can get container IP by `docker inspect`

```bash
$ docker run -d -t --name serf4 tcnksm/serf serf agent -join $(docker inspect --format  '{{ .NetworkSettings.IPAddress }}' 06b0325f6373):7979
```

### Join cluster from docker host (Vagrant)

You need to get container ID which you want to join

```bash
$ serf agent -join $(docker port 06b0325f63 7979)
```

Check members

```bash
$ serf members
precise64     10.0.2.15:7946   alive
2912f653a2e5  172.17.0.3:7979  alive
06b0325f6373  172.17.0.4:7979  alive
108c21eb149a  172.17.0.2:7979  alive
e650af12b2f1  172.17.0.5:7946  alive
```
