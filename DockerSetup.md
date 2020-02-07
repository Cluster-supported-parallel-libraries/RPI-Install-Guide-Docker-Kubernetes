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
