# useful-one-liner
Collection of one-liner commands useful for daily work

## Get latest image name on pierone
```bash
pierone latest --url registry.opensource.zalan.do/ stups ubuntu
```

## Build docker locally and test
```
docker build -t mydocker1 .
docker run -it mydocker1
```

## Delete all containers in docker
```bash
docker rm $(docker ps -a -q)
```
## Delete all images in docker
```bash
docker rmi $(docker images -q -a)
```

## Get number of CPU in bash
```bash
grep -c ^processor /proc/cpuinfo
```

## For deploying a dockerfile
```bash
git clone --depth=1 --branch=master git@github.bus.zalan.do:hxiao/research-lab-docker.git && rm -rf !$/.git && ./research-lab-docker/deploy.sh && rm -rf ./research-lab-docker/
```

## Check GPU usage
```bash
watch nvidia-smi
```

## Fetch article data from Solr
```bash
curl -s -k "https://solr-01-eu-central-1.mop.zalan.do/ad01/select?indent=on&version=2.2&q=*%3A*&fq=&start=0&rows=2&fl=*&qt=&wt=json&explainOther=&hl.fl="
```

## Spawn a new stack from a EC2 (use `--force`)
```bash
senza create senza.yaml 44 29ab124 --disable-rollback --region eu-west-1 --force
```

## Get used size of directories
```bash
du -ah | sort -nr | head
```

## Get free size of a directory
```bash
df -h
```

## Grep the tail of a file
```bash
 tail -f /var/log/application.log | grep --line-buffered "validation"
```

## One-liner dashboard for training Tensorflow
```bash
watch -tcd "nvidia-smi | sed -n 7,10p && free -m && printf '%80s\n' | tr ' ' -&& tail -n 6 /var/log/application.log | grep -Po '\[INFO\]\s\K.+' && printf '%80s\n' | tr ' ' - && cat /var/log/application.log | grep -Po '.\K{".+Config".+' | sort -u && cat /var/log/application.log | grep -Po "\[INFO\].+total.+" | sort -u && printf '%80s\n' | tr ' ' -&& cat /var/log/application.log | grep -Po '\[INFO\]\s\K.+lr.+' | head -n 8 && printf '%80s\n' | tr ' ' -&& cat /var/log/application.log | grep -Po '\[INFO\]\s\K.+lr.+' | tail -n 8"
```

## One-liner dashboard for benchmarking scikit-learn
```bash
watch -tcd "mpstat -P ALL && printf '%80s\n' | tr ' ' -&& free -m && printf '%80s\n' | tr ' ' -&& tail -n 10 /var/log/application.log | grep -Po '\[INFO\]\s\K.+' && printf '%80s\n' | tr ' ' -&& cat /var/log/application.log | grep -Po 'done\!\s\K.+'"
```

## Get list of violations
```bash
fullstop list-violations -l 100000 -s 800d -x '.*rl-andreas.*'
```

## Find all zombie processes and list their dependencies tree
```bash
ps aux | grep 'Z'
pstree -p -s PID
```

## Build a server-client pipe using netcat
```bash
nc -lk 1234
echo 'hdasdaello' | nc 127.0.0.1 1234
```

## Enlarge the image in Tensorboard
```javascript
$('.card.style-scope.tf-panes-helper').style.width = "auto"
```

## Check Tensorflow version
```bash
python -c 'import tensorflow as tf; print(tf.__version__)'  # for Python 3
```

## Convert Tensorboard images to GIF
```bash
python tf_save_images.py log/20170721-001423/valid/events.out.tfevents.1500588867.HandeMacBook-Pro.local Visualization/input_vs_output/image --prefix images/valid-batch
convert images/valid-batch*.png -scale 630x300 images/mnist-valid.gif
```

## Download all sku images
```bash
cat ../sku/skuimg.json | jq -r '.picture_url+"\t"+.sku' | pv | parallel --colsep '\t' -P 8 curl 'https://mosaic01.ztat.net/vgs/media/pdp-color-big/'{1} -o '../sku/'{2}'.jpg' -s
```

## Convert all jpegs to binary images
```bash
ls *.jpg -1 | pv | parallel -P 8 convert './'{1} -colorspace gray -edge 10 -fuzz 1% -threshold 75% -trim -gravity center -extent 60x60 -negate -resize 28x28 './bw/{1}'
```

## Randomly select files and move to a directory
```bash
shuf -n 2000 -e * | xargs -i mv {} test/
```

## Display colored json with less
```bash
cat skucat.json | jq . -c -C | less -r
```

## Solving SSH password problem on Mac
```bash
ssh-add -K ~/.ssh/ghe_rsa.pub
```

## Specific version when using pip install
```bash
 pip install -U scikit-learn==0.19b2
```

## Delete local repos not exist on remote
```bash
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
```

## Convert mov to gif
```bash
ffmpeg -i test.mov -filter:v scale=800:-1 -pix_fmt rgb24 -r 10 -f gif - | convert -delay 5 -layers Optimize -loop 0 - test.gif
```

## Masking entire IPv4 address
```
0.0.0.0/0
```

## Show where model is saved
```bash
grep -Po '\[INFO\]\smodel\ssaved\sin\sfile:\s\K.+' /var/log/application.log | head -1
```

## Bash into the docker container
```bash
docker exec -it $(docker ps -a --no-trunc -q) bash
```

## Get number of trainable variables
```python
np.sum([np.prod(v.shape) for v in tf.trainable_variables()])
```

## Get the rank of an array
```python
(-np.array([1,4,2,3])).argsort().argsort()
array([3, 0, 2, 1])
```

## Check the evaluation using jq
```bash
cat /mounts/opt/workspace/log/*/save/eval.json | jq '. | select(.dataset=="test1") | .["map"]'
```

## Shuffling a training batch
```python
def shuffle_batch(a_batch):
    return {k: v for k, v in zip(a_batch.keys(), utils.shuffle(*a_batch.values()))}

a = {'da': [1, 2], 'a': [2, 3], 'c': [3, 4], 'd': [5, 6]}
print(shuffle_batch(a))
```

## Debug the last docker image with bash
```bash
docker run --entrypoint /bin/bash -i -t $(docker images -q | head -n1)
```

## Run job in background
```bash
nohup python /path/to/test.py &
```

## Count word frequencies using dask
```python
wordcount = b.str.split().flatten().frequencies().topk(10, lambda x: x[1])
```

## Get CUDA version
```bash
nvcc --version
cat /usr/local/cuda/version.txt
```

## Get CuDNN version
```bash
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2 
```

## Get Python 3.x path in pyenv
```bash
pyenv which python3.6
```

## Show two files side by side
```bash
pr -w $COLUMNS -m -t <(cat dev-0321-212403.json | jq .pred_answer -r) <(cat dev-0321-205902.json | jq .pred_answer -r) | less -N
```

## Store git credentials
```bash
git config credential.helper store
```

## Show diff in color & side-by-side
```bash
icdiff <(cat dev-0321-212403.json | jq .pred_answer -r) <(cat dev-0321-205902.json | jq .pred_answer -r) | less -N
```

## Show who killed my process
```bash
dmesg -T| grep -E -i -B100 'killed process'
```

## Reset a GPU core
```bash
nvidia-smi --gpu-reset --id=1
```

## Crawling product info
```bash
curl 'https://pg.m.tmall.com/shop/shop_auction_search.do?spm=a1z60.7754813.0.0.586e7bc0VkVRiq&suid=217101303&sort=s&p=2&page_size=12&from=h5&shop_id=57895461&ajson=1&_tm_source=tmallsearch&callback=jsonp_39523892' -H 'accept-encoding: gzip, deflate, br' -H 'accept-language: en-US,en;q=0.9,ja;q=0.8,zh-CN;q=0.7,zh;q=0.6,zh-TW;q=0.5,de;q=0.4,fr;q=0.3' -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36' -H 'accept: */*' -H 'referer: https://pg.m.tmall.com/shop/shop_auction_search.htm?spm=a1z60.7754813.0.0.586e7bc0VkVRiq&suid=217101303&sort=default' -H 'authority: pg.m.tmall.com' -H 'cookie: t=9d87528e8cb0a98ec2e7c2aca598ca2e; _tb_token_=efeebc6181054; cookie2=14e148bf4ada601074ff9c579734c1f5; cna=5xAtE2CZhiUCAQ4RFiTT2tHP; _m_h5_tk=8ed274719ae8756ca310037727728164_1522385620808; _m_h5_tk_enc=a785d68cd0ab1c2b8a440a1f06f2836c; isg=BODgWkxurp6vVhKas-B5DxY2sei4rKeamlU5yFrxKvudVYJ_AviMQ4Qr6fVVYnyL' --compressed
```

## Force pull
```bash
git reset --hard HEAD & git pull
```

## Repeat an array in tensorflow
array([1, 2, 3]) -> array([1, 1, 2, 2, 3, 3])
```python
tf.reshape(tf.tile(tf.expand_dims([1, 2, 3], -1),  [1, 2]), [-1])
```

## Faster load large csv matrix using pandas
```python
import pandas as pd
a = pd.read_csv('/data/cips/data/larryjf-v2/passages/embedding.vec', engine='c', sep=' ', index_col=False, header=None).values
```

## Print specific lines using sed
```bash
sed -n '4p;1026241p;6775004p' passage-text.txt
```
