# flask-aws-template
This application showcases a bare-bones Flask application that can be easily deployed to AWS Elastic Beanstalk.
The application is a fully responsive PWA website with some useful features that many applications require e.g. user authentication, logging, privacy statements etc.

# Motivation
I started to build a Flask based responsive website to run on AWS. I spent a lot of time searching for the best way to run a fully responsive PWA website built in Flask running on AWS. I could not find a complete working example to use as a starting point. I've decided to create this repo to showcase the way I did this so others can benefit from my learning.

I also spent a lot of time building some of the "compliance/hygiene" type pages like Contact Us, Privacy Policy etc. that every website requires. I thought it would be useful to have a working Flask application that has these plumbed in and ready for enhancements. This means less time on those type of features and more on the value-add product differentiators you really want to be working on.

Another motivation is I believe in using SaaS services and not re-inventing the wheel especially when it comes to super-sensitive topics like User Authentication and Sign-Up. There is no need to be building yet-another-authentication piece of code when you can trust companies like Auth0 to do this. Far better to plug into SaaS offerings like Auth0 that provide commodity services so you can once again focus on the value-adds.

Furthermore, it's not only features that you need to care about. It's also SEO, tracking product analytics etc. This is why I've also added the use of Google Analytics tracking and various tags/best practice to assist with SEO so people actually find your site.

And finally...... good engineering practices are a must. CI/CD and code quality are critical. Why spend ages trying to construct a pipeline that builds, tests and deploys the application when you can re-use an existing one. Learn how to use tools such as CircleCI, SonarCloud and Snyk to help improve your application. This sample app shows some of these tools in use.

# Status
The application is fully working.

Check it out by forking the repo and running locally.

Note that whilst I've beem building apps for AWS, this application will work on other cloud platforms of your choice. It has [Docker](Dockerfile) and [docker-compose](docker-compose.yml) files available that can be utilised to run on other platforms.

# Technology
It's built in Python leveraging the [Flask](https://flask.palletsprojects.com/en/1.1.x/#) framework. 
I've been using Python 3.7.7.

It requires a database backend. I've tested it under MySQL. Database access is performed via SQLAlchemy so theoretically other databases should work providing the relevant driver is installed.

The front-end uses the [Bootstrap](https://getbootstrap.com/) framework in order to provide a responsive UI. It works on mobile browsers as well as traditional Chrome, Safari and Brave on the desktop.
[FontAwesome](https://fontawesome.com) is also used to provide icons.

The [Auth0](https://auth0.com/) website is used for authentication. I use the Python Authlib package to assist with the authentication.

[Google Analytics tracking](https://marketingplatform.google.com/about/analytics/) is supported in all of the HTML pages.

It is a fully [PWA compliant](https://web.dev/progressive-web-apps/) application so can be readily installed on Android and iOS devices. [Lighthouse](https://developers.google.com/web/tools/lighthouse/) in the Chrome DevTools has been used to test the compliance of the application.

The [requirements.txt](requirements.txt) file contains the dependent python packages to be installed.

# Features

This app contains the following features:
* Use of Bootstrap and FontAwesome to produce a fully responsive, good-looking website
* A homepage and navbar built from Bootstrap
* Static pages required by many sites (FAQ, disclaimer, cookie policy, privacy policy)
* Contact Us page showcasing how to use AWS S3 to store the user submitted data
* Sign-up/Log-in/Log-out using Auth0 as the authentication provider
* A fully responsive PWA application that runs as well on the desktop as it does on a mobile device
* Database access via SQLAlchemy so agnostic to underlying database used
* Google analytics
* Use of a Content Distribution Network (CDN) to serve the static content
* Pages contain the metadata required by search engines
* Metadata files required for PWA applications (manifest.json, service-worker.js, offline.html)
* Robust server-side error logging
* Showcases the use of CircleCI for a full CI/CD pipeline that runs automated tests and will deploy to AWS


# Installation

It's a standard Flask app with a database backend. To install and run locally:

## Run locally (non-Docker)

### Step 1 -  ensure Python is installed. 

It's been tested with Python 3.7.7.

### Step 2 - Fork this repo

Fork this repo, enter the following on the command line in the folder where you wish the code to live
```commandline
git clone <path to repo>
```

Now change into the directory
```commandline
cd flask-aws-template
```

### Step 3 - install the dependencies in a virtual environment

create the virtual environment
```commandline
python -m venv .venv
```

now activate the virtual environment
```commandline
source ./.venv/bin/activate
```

install the requirements
```commandline
pip install -r requirements.txt
```

### Step 4 - Set-up the initial environment variables

Some environment variables are required to launch the app

```commandline
export FLASK_APP=my_app
```

### Step 5 - Set-up the Flask logger
The logging configuration file [log_config.yaml](log_config.yaml) contains the configuration for the logger. 

It logs messages to a log file contained at `/tmp/my_app.log`. This may not be suitable for your environment so change the value in this file.

### Step 6 - set-up a database

A database is required. It will default to sqlite which is installed by default on many OS's.
Once you have a database, update the connection string. Otherwise, it will default to a local sqlite one which is fine for development.

```commandline
export DEV_DATABASE_URL=<YOUR DATABASE CONNECTION STRING>
```

### Step 7 - initialise the database

SQLAlchemy and Alembic are used to initialise the database

```commandline
flask db upgrade
```

### Step 8 - Launch the app

You can now run the app

```commandline
flask run
```

You should see a statement on the command line indicating the app is now ready at localhost:5000

Open up a browser to `http://localhost:5000`. Click around. Not all functionality will be working just yet as you need to register with some providers

### Step 9 - Set-up user authentication
The app uses [Auth0](https://auth0.com/) as its user authentication provider. You need to register for a free account with Auth0. Follow the guidance here to not only set-up your free account but also configure your tenant.

The document [Authentication](docs/authentication.md) describes the Auth0 setup in further details along with an explanation of how the app has been coded to use Auth0.

The important data required, once you have registered and have a tenant, is the CLIENT_ID, CLIENT_SECRET, CLIENT_DOMAIN. Flask needs these set-up as environment variables. Execute the following

```commandline
export AUTH0_CLIENT_ID=<YOUR_CLIENT_ID>
export AUTH0_SECRET_ID=<YOUR_SECRET_ID>
export AUTH0_CLIENT_DOMAIN=<YOUR_DOMAIN>
```
If you restart the application with `flask run`, the sign-up, log-in and log-out links will now work.

### Step 10 - (Optional) Set-up Google analytics

If you have a web-domain and wish to use [Google Analytics](https://analytics.google.com/) for this Flask apo, then you will need to register with google. Follow their guidance here.

Once you've done this, set-up the relevant environment variable

```commandline
export GA_CODE=<YOUR_GOOGLE_TAG>
```

### Step 11 - (Optional) Set-up an AWS S3 bucket to store feedback from the Contact Us page

To fully use the contact-us page, you will need an AWS S3 bucket. Once you have one then set-up the following environment variables:

```commandline
export CONTACT_US_FORMAT=S3
export S3_BUCKET=<NAME_OF_BUCKET
```

### Step 12 - (Optional) Set-up a CDN to serve static content

If you'd like to use a CDN to serve the static content then register and set-this up. Once you have the URL for the CDN then you need to let Flask know.

```commandline
export CDN_DOMAIN-<YOUR_CDN_DOMAIN>
```

### Finally
If you've followed the previous steps then you will have a fully running Flask instance

## Run via Docker
Or you can use the [Dockerfile](Dockerfile) and [docker-compose file](docker-compose.yml) to launch the Flask app in a container connected to MySQl running in a container. 

Make sure Docker is running locally and then enter:

```commandline
docker-compose build
```

followed by

```commandline
docker-compose up
```

Follow the instructions in the previous section for setting up Auth0, Google Analytics, AWs S3 bucket and your CDN. The environment variables will need to be set-up locally as well as the docker-compose command passes the local environment variables to the container.

## Running in the cloud

This application will readily run in AWS and, as a bonus, on the free tier. I've used:
* Elastic Beanstalk to run the Flask application
* The Flask applications connects to RMS with a MySQL database
* S3 to store the Contact Us queries as json files
* Route 53 to route requests to a load-balancer in front of Elastic Beanstalk 
* Cloudfront as a CDN to serve the application's static files (images, css, js) from a dedicated S3 Bucket

CircleCI is used to automatically deploy the application to Elastic Beanstalk and the static files to S3.

[Running on AWS](docs/runonaws.md) describes the AWS set-up required to run the Flask app on the AWS Elastic Beanstalk PaaS.

## Tests and CI/CD

### Tests
The tests folder contains all of the application tests.
Selenium is used in headless mode to test the UI. [Chromedriver](https://sites.google.com/a/chromium.org/chromedriver/downloads) will need to be installed.

To run the tests execute `Make test` on the command line.
You can also run various linters by entering `Make lint`. These will provide some errors due to the way Flask works.

### CI/CD

I've leveraged [CircleCI](https://circleci.com) for Continuous Integration and Continuous Deployment. Tests are automatically run the against a MySQL database. Builds on master are automatically deployed when all tests pass successfully. Check out the [config file](.circleci/config.yml)

I've also used [SonarCloud](https://sonarcloud.io) to highlight code coverage and code quality issues.

[Snyk](https://snyk.io) is used to scan the requirements.txt file for potential vulnerabilities.

## How to use?
Take this code as is and alter to better suit your needs.
* You can add new imagery and styles. Alter the files in the [static](app/static) folder
* Change the [templates](app/templates)
* Change the [code](app) 
* update the meta data in the [base template](app/templates/base.html) to better support your site

## Contribute
Feedback and contributions are welcome. Checkout the [code of conduct](CODE_OF_CONDUCT.md) and [contributor guidelines](CONTRIBUTING.md).

## To dos
* The [issue tracker](https://github.com/greendinosaur/flask-aws-template/issues) contains the proposed enhancements and bug fixes.

# License
This is licensed under a MIT licence.

