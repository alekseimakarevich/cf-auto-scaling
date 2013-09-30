Stupid simple scripts for automaticaly scale applications hosted in Cloudfoundry


### Requiremts
- Ruby 1.9.3 +
- Apache Benchmark (http://httpd.apache.org/docs/2.2/programs/ab.html)
- Some CF based PaaS public/private provider with full access to the Cloud Controller API

### Set up

#### Clone the repo
```
git clone <this repo CLONE URL >
cd cf-auto-scaling
bundle
```

#### Set up credentials
```
cp config/config.yml.template config/config.yml
```
Replace values in config.yml to fit your context

#### Push demo app
I've created a simple sinatra app that generate secure random number which require CPU (I am very new at this. 
Sorry for my ignorance)
> You need to be targeted and logged in into a CF PaaS

Modify or delete manifest.yml if required

```
cd cd sinatra-test-app/
cf push
```


### Auto-Scaling script
Just run 
```
./auto_scale.rg
```
This script runs forever. Just CTR + C it when you are done
> Current version takes first app from your first space. Yes, it sucks.. quick and dirty, sorry


### Throw load to the app
Lets make 5000 requests with 20 requests at a time
```
ab -n 5000 -c 20 http://sinatra-manu.cfapps.io/
```

### Whatch it working
First output
```
Cloudfoundry target: https://api.run.pivotal.io
Authenticating with user: <your username defined in config/config.yml>
Fetching first space...
Got space: development

Fetching first app from space development ...
Got app: sinatra-manu (sinatra-manu.cfapps.io)

CPU threshold: 50%
------------------
App sinatra-manu stats:
-- Instances: 1
-- AVG CPU load: 0.00 %
-- AVG Memory: 25.91 MB
```

When requests are hitting harder, lets suppose your instane is running out of CPU power, you'll get something
like this
```
App sinatra-manu stats:
-- Instances: 1
-- AVG CPU load: 100.00 %
-- AVG Memory: 25.91 MB

------------> AVG CPU Load went over threshold (50), scaling app by 1 instance
Update successful
App sinatra-manu stats:
-- Instances: 2
-- AVG CPU load: 50.00 %
-- AVG Memory: 25.90 MB

------------> AVG CPU Load went over threshold (50), scaling app by 1 instance

App sinatra-manu stats:
-- Instances: 3
-- AVG CPU load: 33.00 %
-- AVG Memory: 24.32 MB

App sinatra-manu stats:
-- Instances: 3
-- AVG CPU load: 33.00 %
-- AVG Memory: 25.79 MB


App sinatra-manu stats:
-- Instances: 3
-- AVG CPU load: 0.00 %
-- AVG Memory: 25.61 MB
```
___
You are welcome to contribute via [pull request](https://help.github.com/articles/using-pull-requests).
