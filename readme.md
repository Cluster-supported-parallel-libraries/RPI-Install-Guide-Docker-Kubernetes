# RPISetup
### Connect til Raspberry Pi
- Enable SSH, enten ved at lave en tom fil i microSD-kortet som hedder SSH
- ELLER, hvis du connecter til RPI igennem HDMI, kan du enable SSH ved at skrive:
```sh
$ sudo systemctl enable ssh
$ sudo systemctl start ssh
```
### Get Raspberry Pi IP
```sh
$ cat /var/lib/misc/dnsmasq.leases
```

### Opdater Raspberry Pi
```sh
$ sudo apt-get update
$ sudo apt-get upgrade
```

### Installer docker engine Raspberry Pi
Link til hjemmeside: https://dev.to/rohansawant/installing-docker-and-docker-compose-on-the-raspberry-pi-in-5-simple-steps-3mgl?fbclid=IwAR15V0ArXn00x7a4Dn3oQkmqzfQma48PslYZX35khxu8_YwYPYaIPE7IuPA
```sh
$ sudo curl -sSL https://get.docker.com/ | sh
```
Test lige om du kan køre:
```sh
$ docker version
```

Hvis den broker sig over 'got permission denied', så kør følgende kommando:
```sh
$ sudo usermod -aG docker 'dit username'
```
Genstart efter

#### Installer .NET Core SDK og .NET Runtime
Link til hjemmeside: https://dotnet.microsoft.com/download/dotnet-core/3.1
Download de pakket filer (husk 32-bit):
```sh
brug følgende link til .NET core SDK
arm 32 : https://download.visualstudio.microsoft.com/download/pr/d52fa156-1555-41d5-a5eb-234305fbd470/173cddb039d613c8f007c9f74371f8bb/dotnet-sdk-3.1.101-linux-arm.tar.gz
arm 64 : https://download.visualstudio.microsoft.com/download/pr/cf54dd72-eab1-4f5c-ac1e-55e2a9006739/d66fc7e2d4ee6c709834dd31db23b743/dotnet-sdk-3.1.101-linux-arm64.tar.gz

$ wget 'link fra hjemmeside (.NET Core SDK)'

brug følgende link til .NET Runtime
arm 32 : https://download.visualstudio.microsoft.com/download/pr/98931269-612c-47cd-a5a1-f1d8e616c950/1ba015724bba919eccbf159dbda0a483/dotnet-runtime-3.1.1-linux-arm.tar.gz
arm 64 : https://download.visualstudio.microsoft.com/download/pr/38325910-0157-4f3a-b093-da799dcaa24b/d4892d3a53a6d917fbab4037624181a9/dotnet-runtime-3.1.1-linux-arm64.tar.gz

$ wget 'link fra hjemmeside (.NET Runtime)'
```
Lav et direktory som dine filer pakkes ud til:
```sh
$ mkdir dotnet-arm32
```

Pak dine .NET Core SDK og .NET Runtine ud:
```sh
$ tar zxf 'navnet på din downloaded .NET CORE SDK fil' -C 'stien til ny-lavet mappe'
$ tar zxf 'navnet på din downloaded .NET Runtime' -C 'stien til ny-lavet mappe'
```

Exportere filstien til din globale enviroment variable (OPS, platform arm32 eller amd64):
```sh
$ export DOTNET_ROOT=/home/master/dotnet-arm32
$ export PATH=$PATH:$HOME/dotnet-arm32
```

Exportere din enviroment variable til at peristere
```
sudo nano /etc/profile
```
add til nederst til filen:
```
export DOTNET_ROOT=/home/master/dotnet-arm32
export PATH=$PATH:$HOME/dotnet-arm32
```


Test med:
```sh
$ dotnet --info
```

### Docker setup med experimental features
- Højreklik på docker og åben instillinger
- Navigere ind til Docker engine  tabben
- Sæt experimental til true, fra false

```sh
 {
  "experimental": true,
  "debug": true
}
```
- Gå ind i tabben lige under experimental (Command line) og enable 'Enable experimental features'
- Med experimental features enabled, kan vi bruge buildx commandoen med docker
- Hjemmesiden med buildX https://docs.docker.com/buildx/working-with-buildx/

Vi ønsker at bygge til en arm processor. Hertil skal vi bygge vores projekt med buildx med den valgte platform arm.

```sh
docker buildx build --platform linux/amd64,linux/arm/v7 -t 'username'/'projektName':'version' . --load 
```
- Grunden til --load argumentet, er da vores image til tider ikke bliver loaded ind, men ligger i cache
- Og til sidst pusher vi til vores dockerhub repo
```sh
docker push 'username'/'projektName':'version'
```

For at køre image i docker på Pi:
```sh
docker run -p 8080:80 'username'/'projektName':'version'
```
[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
