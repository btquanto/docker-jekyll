# Docker-Jekyll

Running Jekyll with a small-size docker container.
The image size is only 226.5MB

# Usage

1. Serve the current folder
    ```
    docker run -d --name jekyll  -v `pwd`:/src -p 4000:4000  btquanto/docker-jekyll jekyll serve -H 0.0.0.0
    ```
    Add `--draft` if you want drafted posts to also be included
    ```
    docker run -d --name jekyll  -v `pwd`:/src -p 4000:4000  btquanto/docker-jekyll jekyll serve -H 0.0.0.0 --draft
    ```
    You may want to replace `` `pwd` `` with the path of your choice
    Your site will be available at [localhost:4000](http://localhost:4000)

2. Using `docker-compose`
    ```
    version: '2'

    services:
      jekyll:
        image: btquanto/docker-jekyll
        ports:
          - 4000:4000
        volumes:
          - ./:/src
        command: jekyll serve -H 0.0.0.0
    ```

3. Installing new gems
    * Method 1
        * Run a new container with `/bin/sh` command
        ```
        docker run -dt --name jekyll -v `pwd`:/src -p 4000:4000 btquanto/docker-jekyll /bin/sh
        ```
        * Install new gems, for example, `redcarpet`
        ```
        docker exec jekyll gem install redcarpet
        ```
        * Test if any more gems missing
        ```
        docker exec jekyll serve -H 0.0.0.0
        ```
        * Commit the container into a new image
        ```
        docker commit jekyll my_jekyll_image
        ```
        * Start a new container from the newly built image
        ```
        docker stop jekyll
        docker rm jekyll
        docker run -dt --name jekyll -v `pwd`:/src -p 4000:4000 my_jekyll_image jekyll serve -H 0.0.0.0
        ```

    * Method 2
        * Create a new Dockerfile with the following content
        ```
        FROM btquanto/docker-jekyll
        RUN gem install <list of gems>
        ```
        * Build a new image
        ```
        docker build -t my_jekyll_image
        ```
        * Start a new container from the newly built image
        ```
        docker stop jekyll
        docker rm jekyll
        docker run -dt --name jekyll \
            -v `pwd`:/src -p 4000:4000 \
            my_jekyll_image jekyll serve -H 0.0.0.0
        ```

# Author

* [https://github.com/btquanto](https://github.com/btquanto)
* [http://btquanto.github.io](http://btquanto.github.io)
* [https://fb.me/theitfox](https://fb.me/theitfox)

# License
```
            DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE
                    Version 2, December 2004

 Copyright (C) 2016 Sam Hocevar <sam@hocevar.net>

 Everyone is permitted to copy and distribute verbatim or modified
 copies of this license document, and changing it is allowed as long
 as the name is changed.

            DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE
   TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION

  0. You just DO WHAT THE FUCK YOU WANT TO.
```
