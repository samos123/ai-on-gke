# Benchmarking networking performance on G2

Benchmarking VM to VM connectivity between g2-standard-96 VMs.

For this tests 2 x g2-standard-96 are used, 1 serves as the iperf-server and the other
serves as the iperf-client.

## with virtio
```bash
gcloud container node-pools create g2-standard-96-gvnic --cluster l4-demo \
  --accelerator type=nvidia-l4,count=8,gpu-driver-version=latest \
  --machine-type g2-standard-96 \
  --ephemeral-storage-local-ssd=count=8 \
  --enable-autoscaling --enable-image-streaming \
  --num-nodes=0 --min-nodes 0 --max-nodes 3  \
  --shielded-secure-boot \
  --shielded-integrity-monitoring \
  --node-locations ${REGION}-a,${REGION}-b --region ${REGION}
```


```
iperf -t 300 -c iperf-server -P 16
[ ID] Interval       Transfer     Bandwidth
[  6] 0.0000-300.0170 sec   208 GBytes  5.97 Gbits/sec
[  2] 0.0000-300.0171 sec   269 GBytes  7.70 Gbits/sec
[ 11] 0.0000-300.0171 sec   274 GBytes  7.83 Gbits/sec
[  7] 0.0000-300.0171 sec   248 GBytes  7.10 Gbits/sec
[  1] 0.0000-300.0171 sec  83.2 GBytes  2.38 Gbits/sec
[  9] 0.0000-300.0169 sec  73.6 GBytes  2.11 Gbits/sec
[ 15] 0.0000-300.0170 sec   181 GBytes  5.18 Gbits/sec
[ 12] 0.0000-300.0169 sec   126 GBytes  3.60 Gbits/sec
[  4] 0.0000-300.0168 sec   209 GBytes  5.99 Gbits/sec
[  5] 0.0000-300.0172 sec   217 GBytes  6.21 Gbits/sec
[  8] 0.0000-300.0170 sec   144 GBytes  4.12 Gbits/sec
[ 10] 0.0000-300.0170 sec   180 GBytes  5.14 Gbits/sec
[  3] 0.0000-300.0169 sec   223 GBytes  6.38 Gbits/sec
[ 16] 0.0000-300.0172 sec  81.6 GBytes  2.34 Gbits/sec
[ 14] 0.0000-300.0170 sec  76.4 GBytes  2.19 Gbits/sec
[ 13] 0.0000-301.0174 sec   143 GBytes  4.09 Gbits/sec
[SUM] 0.0000-300.0018 sec  2.67 TBytes  78.3 Gbits/sec
```

## with gVNIC

```bash
gcloud container node-pools create g2-standard-96-gvnic --cluster l4-demo \
  --accelerator type=nvidia-l4,count=8,gpu-driver-version=latest \
  --machine-type g2-standard-96 \
  --ephemeral-storage-local-ssd=count=8 \
  --enable-autoscaling --enable-image-streaming \
  --num-nodes=0 --min-nodes 0 --max-nodes 3  \
  --shielded-secure-boot \
  --shielded-integrity-monitoring \
  --node-locations ${REGION}-a,${REGION}-b --region ${REGION} --enable-gvnic
```

```
[ ID] Interval       Transfer     Bandwidth
[ 16] 0.0000-300.0144 sec   144 GBytes  4.13 Gbits/sec
[ 12] 0.0000-300.0143 sec   159 GBytes  4.56 Gbits/sec
[  6] 0.0000-300.0144 sec   152 GBytes  4.34 Gbits/sec
[  2] 0.0000-300.0142 sec   178 GBytes  5.11 Gbits/sec
[  9] 0.0000-300.0142 sec   185 GBytes  5.29 Gbits/sec
[  7] 0.0000-300.0141 sec   269 GBytes  7.69 Gbits/sec
[ 11] 0.0000-300.0142 sec   137 GBytes  3.92 Gbits/sec
[  8] 0.0000-300.0141 sec   135 GBytes  3.86 Gbits/sec
[ 10] 0.0000-300.0144 sec   161 GBytes  4.62 Gbits/sec
[ 14] 0.0000-300.0143 sec   185 GBytes  5.31 Gbits/sec
[  1] 0.0000-300.0141 sec   267 GBytes  7.63 Gbits/sec
[ 15] 0.0000-300.0142 sec   201 GBytes  5.75 Gbits/sec
[  5] 0.0000-300.0141 sec   203 GBytes  5.81 Gbits/sec
[ 13] 0.0000-300.0143 sec   152 GBytes  4.34 Gbits/sec
[  4] 0.0000-300.0141 sec   192 GBytes  5.51 Gbits/sec
[  3] 0.0000-300.0142 sec   199 GBytes  5.69 Gbits/sec
[SUM] 0.0000-300.0025 sec  2.85 TBytes  83.6 Gbits/sec
```

