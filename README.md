# hapi-swagger

This repository is a fork of the [hapi-swagger](https://github.com/glennjones/hapi-swagger) project developped by glennjones.

[![Build Status](https://travis-ci.org/reptilbud/hapi-swagger.svg?branch=master)](https://travis-ci.org/reptilbud/hapi-swagger)
[![Coverage Status](https://coveralls.io/repos/github/reptilbud/hapi-swagger/badge.svg?branch=master)](https://coveralls.io/github/reptilbud/hapi-swagger?branch=master)

# Install

You can add the module to your HAPI using npm:

    $ npm install @reptilbud/hapi-swagger --save

If you want to view the documentation from your API you will also need to install the `inert` and `vision` plugs-ins which support templates and static
content serving.

    $ npm install inert --save
    $ npm install vision --save

# Documentation

* [Options Reference](optionsreference.md)
* [Usage Guide](usageguide.md)


# Quick start

In your HAPI apps main JavaScript file add the following code to created a HAPI `server` object. You will also add the routes for you API as describe on hapijs.com site.


```Javascript
const Hapi = require('hapi');
const Inert = require('inert');
const Vision = require('vision');
const HapiSwagger = require('hapi-swagger');
const Pack = require('./package');

const server = new Hapi.Server();
server.connection({
        host: 'localhost',
        port: 3000
    });

const options = {
    info: {
            'title': 'Test API Documentation',
            'version': Pack.version,
        }
    };

server.register([
    Inert,
    Vision,
    {
        'register': HapiSwagger,
        'options': options
    }], (err) => {
        server.start( (err) => {
           if (err) {
                console.log(err);
            } else {
                console.log('Server running at:', server.info.uri);
            }
        });
    });

server.route(Routes);
```

# Tagging your API routes
As a project may be a mixture of web pages and API endpoints you need to tag the routes you wish Swagger to
document. Simply add the `tags: ['api']` property to the route object for any endpoint you want documenting.

You can even specify more tags and then later generate tag-specific documentation. If you specify
`tags: ['api', 'foo']`, you can later use `/documentation?tags=foo` to load the documentation on the
HTML page (see next section).

```Javascript
{
    method: 'GET',
    path: '/todo/{id}/',
    config: {
        handler: handlers.getToDo,
        description: 'Get todo',
        notes: 'Returns a todo item by the id passed in the path',
        tags: ['api'], // ADD THIS TAG
        validate: {
            params: {
                id : Joi.number()
                        .required()
                        .description('the id for the todo item'),
            }
        }
    },
}
```

Once you have tagged your routes start the application. __The plugin adds a page into your site with the route `/documentation`__,
so the the full URL for the above options would be `http://localhost:3000/documentation`.



# Contributing

Read the [contributing guidelines](https://github.com/glennjones/hapi-swagger/blob/master/.github/CONTRIBUTING.md) for details.




# Thanks
I would like to thank all that have contributed to the project over the last couple of years. This is a hard project to maintain, getting HAPI to work with Swagger
is like putting a round plug in a square hole. Without the help of others it would not be possible.


