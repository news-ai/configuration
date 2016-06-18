1. Install golang
2. Install gpm (dependency manager)
3. `wget https://github.com/nsqio/nsq/archive/v0.3.8.zip`
4. `gpm install`
5. `go get github.com/nsqio/nsq/...`

The `~/.bash_profile` should look like:

```
export PATH=$PATH:/usr/local/go/bin
export PATH=$PATH:/home/api/go/bin
export GOPATH=$HOME/go
```

Then start `nsqlookupd` in the background: `nohup nsqlookupd > nsqlookupd.out &`

Then start `nsqd` in the background: `nohup nsqd --lookupd-tcp-address=127.0.0.1:4160 > nsqd.out &`

Then start `nsqadmin` in the background: `nohup nsqadmin --lookupd-http-address=127.0.0.1:4161 > nsqadmin.out &`

To test: `curl -d 'hello world 1' 'http://127.0.0.1:4151/put?topic=test'`

To start syncing to disk: `nohup nsq_to_file --topic=test --output-dir=/tmp --lookupd-http-address=127.0.0.1:4161 > nsq_to_file_topic_test.out &`

Publish more messages to nsqd:

```
curl -d 'hello world 2' 'http://127.0.0.1:4151/put?topic=test'
curl -d 'hello world 3' 'http://127.0.0.1:4151/put?topic=test'
```
