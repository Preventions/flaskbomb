![b191ba7c6456d71b25cb65bbdfd20303.png](https://anonimag.es/i/b191ba7c6456d71b25cb65bbdfd20303.png)

#### Abstract 
* Using the [famous zip bomb concept](https://www.youtube.com/watch?v=jnDk8BcqoR0) *(Silicon Valley S3E07)*, we can send a compressed web-page to the client.  
* The browser will unzip the small compressed page into a very big file, potentially crashing it.  
* This aims to disrupt or crash bots that scan websites to find vulnerabilities. 

#### tldr - Python GZIP-Bomb HTTP Server

>GZip HTTP Bombing in Python for everyone.  

>Uses Python Flask framework  

>Docker friendly
  
>It even has it's own low effort logo.  

>Please keep in mind this is a *counter-measure*.  

>Based on this [excellent piece by Christian Haschek](https://blog.haschek.at/2017/how-to-defend-your-website-with-zip-bombs.html)   

# Flask Bomb  

This repository contains the necessary files to:  
* Host a quick & dirty Flask web server that responds to web requests with a GZip archive as a response page.  

* The recommended way to use FlaskBomb is by deploying it with Docker. You can try it here:  
[![Try with Play-with-Docker](https://anonimag.es/i/c465e530530d506e2264ab9cb37b42f3.png)](http://play-with-docker.com?stack=https://raw.githubusercontent.com/kh4st3x/flaskbomb/master/docker-compose.yml&stack_name=flaskbomb)
-------
### Features:
* Quick and easy
* Fast deployment using Docker
* Lightweight Alpine based Docker container
* Generic code
  * Implement your own rules or payloads !
* (next)User-Agent evasion based on original work
* Choose classic payload generation or faster append generation method    

### Usage:
````bash
docker run -it -p 80:5000 khanon/flaskbomb <normal|fast> <X> # X is the final payload's size in GB  
# Example:  
docker run -it -p 80:5000 khanon/flaskbomb fast 20
````
Default deployment sets options to ```normal 1```  

## Demo - 20G Payloads
#### Standard docker build from git, demo with cURL
[![asciicast](https://asciinema.org/a/141464.png)](https://asciinema.org/a/141464)
------
#### Standard deploy from docker hub, demo with Nikto
[![asciicast](https://asciinema.org/a/141465.png)](https://asciinema.org/a/141465)

## Details & Notes
* Gzip's algorithm enables the possibility to append archives
  * ```normal``` method generates the payload in one single ```dd``` command
  * ```fast``` method generates a 1GB payload and appends itself n times. The final payload is bigger but generates much faster.
* Python 3
* Since this uses Flask's built-in web server, internal port is ```5000```
* The payload is generated using ```gzip``` and ```dd``` on Docker entrypoint.
  * *NIX based for now
  * Full python payload is up for next release
* Flask development server is used. It is recommended to deploy the application on a WGSI + HTTPD
* For those not using docker, generate the payload using the bash commands in ```entrypoint.sh``` and save it as ```./static/cake.gzip``` 
  * The python GZip implementation should make this easier in the future


### Roadmap  
* Evasion  
* Python gzip implementation  
* Load evasion URIs from DirBuster  
* Fingerprinting JS before payload delivery  
