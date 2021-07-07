# RPISetup
### Connect til Raspberry Pi
- Enable SSH, enten ved at lave en tom fil i microSD-kortet som hedder SSH
- ELLER, hvis du connecter til RPI igennem HDMI, kan du enable SSH ved at skrive:
```sh
$ sudo systemctl enable ssh
$ sudo systemctl start ssh
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
$ Docker version
```

Hvis den broker sig over 'got permission denied', så kør følgende kommando:
```sh
$ sudo usermod -aG docker 'dit username'
```
Genstart efter

#### Installer .NET Core SDK og .NET Runtime
Link til hjemmeside: https://dotnet.microsoft.com/download/dotnet-core/3.0
Download de pakket filer (husk 32-bit):
```sh
$ wget 'link fra hjemmeside (.NET Core SDK)'
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

Exportere filstien til din globale enviroment variable:
```sh
$ export DOTNET_ROOT=$HOME/dotnet-arm32
$ export PATH=$PATH:$HOME/dotnet-arm32
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
docker buildx build --platform linux/arm/v7 -t 'username'/'projektName':'version' . --load 
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
