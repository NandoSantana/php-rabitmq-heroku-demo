# php-rabitmq-heroku-demo


Defining process types
Your application now consists of two independent parts: the web interface inside www/, and the worker script inside bin/.

To tell Heroku how to execute both of these, you create a Procfile in the root of the application that contains a “web” and for a “worker” process type:

web: vendor/bin/heroku-php-apache2 www/
worker: php bin/worker.php
Don’t forget to git add Procfile and then git commit.

Deploying
You’re now ready to deploy your application. You will first heroku create a new application and enable the add-ons your application needs so it has a RabbitMQ server, a Redis database and the Bomberman API to talk to:

heroku create
Creating obscure-peak-8422... done, stack is heroku-18
https://obscure-peak-8422.herokuapp.com/ | https://git.heroku.com/obscure-peak-8422.git
Git remote heroku added

heroku addons:create cloudamqp
Creating cloudamqp-round-6277... done, (free)
Adding cloudamqp-round-6277 to obscure-peak-8422... done
Setting CLOUDAMQP_URL and restarting obscure-peak-8422... done, v3
Use `heroku addons:docs cloudamqp` to view documentation.

heroku addons:create heroku-redis
Creating redis-corrugated-5951... done, (free)
Adding redis-corrugated-5951 to obscure-peak-8422... done
Setting REDIS_URL and restarting obscure-peak-8422... done, v4
Instance has been created and will be available shortly
Use `heroku addons:docs heroku-redis` to view documentation.

heroku addons:create bomberman
Creating bomberman-cylindrical-2538... done, (free)
Adding bomberman-cylindrical-2538 to obscure-peak-8422... done
Setting BOMBERMAN_API_KEY and restarting obscure-peak-8422... done, v5
You have successfully added Bomberman with the 'tnt' plan to your Heroku App!
Use `heroku addons:docs bomberman` to view documentation.
Finally, you can deploy the application:

git push heroku master

remote: -----> PHP app detected
remote: -----> No runtime required in 'composer.json', defaulting to PHP 5.6.12
remote: -----> Installing system packages...
remote:        - PHP 5.6.12
remote:        - Apache 2.4.10
remote:        - Nginx 1.6.0
remote: -----> Installing PHP extensions...
remote:        - bcmath (composer.lock; bundled)
remote:        - mbstring (composer.lock; bundled)
remote:        - zend-opcache (automatic; bundled)
remote: -----> Installing dependencies...
remote:        Composer version 1.0.0-alpha10 2015-04-14 21:18:51
remote:        Loading composer repositories with package information
remote:        Installing dependencies from lock file
remote:
remote:          ...
remote:
remote:        Generating optimized autoload files
remote: -----> Preparing runtime environment...
remote: -----> Discovering process types
remote:        Procfile declares types -> web, worker
remote:
remote: -----> Compressing... done, 80.8MB
remote: -----> Launching... done, v6
remote:        https://obscure-peak-8422.herokuapp.com/ deployed to Heroku
Scaling
Right now, the application only runs one dyno with the “web” process type to serve the UI. You also need to run at least one worker to process jobs on the queue, so scale up the worker:

heroku ps:scale worker=1
Scaling dynos... done, now running worker at 1:Free.
heroku ps
=== web (Free): `vendor/bin/heroku-php-apache2 www/`
web.1: up 2015/08/28 11:29:47 (~ 1m ago)

=== worker (Free): `php bin/worker.php`
worker.1: up 2015/08/28 11:31:23 (~ 10s ago)
You may also do this through the Dashboard - find the app you deployed, and make sure both the “web” and “worker” dynos are enabled under the Resources tab.
