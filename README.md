# ImDead

ImDead is a simple web server that serves a 503 (or any other code) with the possibility to customize the page displayed for each host, this is was built for the single purpuse of displaying error pages for the NginxProxyManager software

## Configuration
the little python script that manage that can take 3 environement variable:
- IMDEAD_HOMEPAGE sets the url for the homepage button
- IMDEAD_DEFAULT_TEMPALTE sets the name of the default template
- IMDEAD_RESPONSE_CODE sets the response code

If you want to have a custom page, just create a file `mysuperdomain.fr.html` in the websites folder (or in the mounted volume)

## Run
You can run it like this:
`docker run --publish 5000:80 -v /root/tests/mnt:/app/websites -d thestaticturtle/imdead`

Or if you prefer in a docker-compose:
```yml
version: '2'
services:
  dead:
    restart: unless-stopped
    image: 'thestaticturtle/imdead'
    environment:
      IMDEAD_HOMEPAGE: 'https://thestaticturtle.fr'
      IMDEAD_DEFAULT_TEMPALTE: "default.html"
      IMDEAD_RESPONSE_CODE: 503
    volumes:
      - ./data/dead-websites:/app/websites
```
If you integrate this with nginx proxy manager disableng a site become as simple as redirecting to the container name

![image](https://user-images.githubusercontent.com/17061996/126913335-190ba6ca-1c61-4618-ad12-7e1879f1ac08.png)

Full config example: https://hastebin.com/uqodimaqud.yml

## Template
The server is based on flask and render using templates this means that you can use variables inside the .html file:
- domain = current domain (Equivalent to the host header)
- domain_path = path of the url (Eg visiting http://lol.fr/azert/qwerty would be equal to "azert/qwerty")
- useragent = useragent of the browser
- homepage = the homepage set by the env variable

## Screenshot
![image](https://user-images.githubusercontent.com/17061996/126913563-3f019692-33b9-4ebf-aa48-f3ceff727a09.png)
