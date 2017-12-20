# mydockerfiles

dockerfiles for several cryptocurrencies

- For litecoin I use uphold/litecoind
- For monero I use raboo/monero

For NOMP you could use canercandan/nomp

I used to put a `sleep infinity` at the end of my dockerfiles : it's for debug purpose, don't hesitate to remove it

Files are not named "Dockerfile", just rename them, and build them

You can run them with persistent data outside the container like :
```
mkdir /root/galactrum
mv ore galactrum/Dockerfile
cd galactrum
docker build -t galactrum .
mkdir /data/galactrum
docker run -v /data/galactrum:/galactrum -d --name galactrum galactrum
```
