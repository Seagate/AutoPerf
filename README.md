# AutoPerf

AutoPerf is an automation framework for performance testbench built using ansible and shell scripts. It will create a Grafana dashboard monitoring various system & server performance metrices.


## How to run?


Pre-Requisite:

1. Create an empty 'hosts' file in AutoPerf home directory (~/seagate-tools/AutoPerf/)
2. Update details at ./launch_benchmark.conf:
3. Update details at ./roles/autoperf/vars/main.yml
4. Update details at ./roles/autoPerf/files/cortx-benchmark/influxdbDetails.conf

Execute following command to run AutoPerf benchmark as full.
```
ansible-playbook -i hosts deploy_autoperf.yml --extra-vars '{ "BENCHMARK":"s3bench_basic", "CONFIGURATION":"short", "SAMPLE":"5", "SKIPCLEANUP":"no", "KEY":"password", "nodes":{"1": "node1", "2": "node2"} , "clients":{"1": "client1"}}' -v
```
hosts: list of hosts to get added in it
Benchmark: which benchmark to run, options are available

Inputs required from user -
1. dictionary of nodes
2. dictionary of client


One can collect only system stats using following command.
```
ansible-playbook -i hosts deploy_autoperf.yml --extra-vars '{ "BENCHMARK":"NONE", "WAIT":"5" ,"CONFIGURATION":"short", "SAMPLE":"5", "SKIPCLEANUP":"no", "KEY":"password", "nodes":{"1": "node1", "2": "node2"} , "clients":{"1": "client1"}}' -v
```

AutoPerf also includes some sanity checks as below tasks on client:

1. Check S3 endpoint entry is present in '/etc/hosts' file.
2. Check S3 endpoint is reachable.
3. Check 'aws' package is installed.
4. Check 'awscli' is configured.

Pre-requisite step to perform AutoPerf s3 performance testing:

1.	S3Cluster should be configured with latest build.
2.	S3Cluster is stable and in running state.
3.	It must require root access to the S3 client and S3 server.
4.	To enable Passwordless authentication, AutoPerf Dashboard will ask you for the password.
5. 	S3client must be pre-configured. User must be able to perform S3 bucket Operations. (# aws s3 ls)

Note: -
1.	AutoPerf will take care of all configuration like Benchmark tools, autoperf  and pre-requisites packages on S3 Client and S3 Server
2.	While running fio benchmark, you will observe the S3 performance graph (Grafana Dashboard) only on the primary and secondary node of S3 Server respectively. It will not be reflected on your Client server.
3.	For the remaining benchmark, you can observe the s3 performance graph (Grafana Dashboard) only on the S3 Client server.
