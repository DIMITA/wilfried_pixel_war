# wilfried_pixel_war

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Utiliser Docker
```
docker build -t pixel_app .
docker run -it -p 8087:8080 pixel_app
```

l'application es donc démarré sur le port 8087 de votre ordinateur


### Lints and fixes files
```
npm run lint
```

# NB :
L'application se connecte sur un backend sur le port 3008.
Vous pouvez changer ce port dans le fichier .env et relancer l'application

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
