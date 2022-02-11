# Workers with Wrangler

- Docs-> https://workers.cloudflare.com/

# signup/signin into cloudflare

- Open Cloudflare and signup for workers thing

# Installing Wrangler

- next, open VSCode and check whether you have npm installed or not by opening a new terminal with opening any new folder.
    - command to check-> npm/npm -v
- then install wrangler with cloudflare
    - npm install -g @cloudflare/wrangler
    - check with-> wrangler -v
- then go for login, type 
    - wrangler login
    - type "y" to open browser window to authorise
    - then you will be able to see, it is authorise
    - check who is logged in
        - wrangler whoami

- If it doesn't work for you then check with cmd

# starting new project with Wrangler generate command

- https://developers.cloudflare.com/workers/cli-wrangler/commands
- type this command to see what we can do 
    - wrangler generate -h
    - wrangler generate [projectname]
    - go inside your project with-> cd [projectname]
    - ls (list files) in cmd only

# Now hands on to code in index.js

- open index.js from your project
- and inside the terminal type
    - wrangler preview
    - it will open a new tab with the output screen

- inside the index.js file
    - there are basically two events 
        - "fetch"
            - this is a HTTP event
        - "Schedule"
            - which is like scheduled worker, which we can run some code on time basis
    - basically what we are doing we are fetching the request and giving it to handler which is a async handler it will execute our whole request as a parameter and passing a response to that request that has been made.
        - Response
            - it basically contains body "Hello World" kind of
            - other then that we have options, where we can set some headers etc
        - more about response
            - https://developers.cloudflare.com/workers/runtime-apis/response

# preview and publish the project

- goto terminal type
    - wrangler whoami 
        - and copy the account id you want to start with
    - open wrangler.toml in your project
        - wrangler.toml
            - it is a configuration file for workers project
    - wrangler has an inbuilt tool that has capability to understand what your project has to do

    - wrangler dev
        - it will expose a port with local ip to your project so that you can build dynamically
        - ctrl + c to end in terminal
    -  we will create a workers project to run on our account
        - wrangler publish
            - it will basically publish our code to expose a new url with cloudflare
            - go and check into your account it is build and published succesfully

# now let's create a html for rendering on browser

- create new file with [anyname].js in project folder
    - inside that file write function that writtens a string in HTML format
        const template = () => `
            <!DOCTYPE html>
            <html lang="">
                <head>
                    <meta charset="utf-8">
                    <meta http-equiv="X-UA-Compatible" content="IE=edge">
                    <meta name="viewport" content="width=device-width, initial-scale=1">
                    <title>Hello</title>

                    <!-- Bootstrap CSS -->
                    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

                    <!-- HTML5 Shim and Respond.js IE8 support of HTML5 elements and media queries -->
                    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
                    <!--[if lt IE 9]>
                        <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.3/html5shiv.js"></script>
                        <script src="https://oss.maxcdn.com/libs/respond.js/1.4.2/respond.min.js"></script>
                    <![endif]-->
                </head>
                <body>
                    <h1 class="text-center">Hello World from nayan rajani</h1>

                    <!-- jQuery -->
                    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
                    <!-- Bootstrap JavaScript -->
                    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
                </body>
            </html>
        `

        export default template
    
    - import the above file into index.js and make changes
        import template from './mytype'

        addEventListener('fetch', event => {
        event.respondWith(handleRequest(event.request))
        })
        // /**
        //  * Respond with hello worker text
        //  * @param {Request} request
        //  */
        async function handleRequest(request) {
        return new Response(template(), {
            headers: { 'content-type': 'text/html' },
        })
        }
    - one more change in wrangler.toml
        - from type = "javascript" to type = "webpack"
            - it will make our project to accept other packages from our projects or from npm
    - now just hit the wrangler publish again 
        - it will do a little more building

# Render Cloudflare Region Data for a Request Using request.cf

- https://developers.cloudflare.com/workers/runtime-apis/request
- Now we are going to perform some task related to request.cf
- we will be changing something in index.js and mytype.js(template)
- index.js
    - passing the argument inside the template(request.cf)

        async function handleRequest(request) {
            return new Response(template(request.cf), {
                headers: { 'content-type': 'text/html' },
            })
        }

- mytype.js
    - passing the same in template function, we want to display the country code and emoji from where the user belong
    - for that we need to install a package
        - npm i country-code-emoji
        - then import it 
            - import flag from country-code-emoji

            const template = (cf) => {
                const emoji = flag(cf.country) || `from nayan rajani`
                return `
            <!DOCTYPE html>
            <html lang="">
                <head>
                    <meta charset="utf-8">
                    <meta http-equiv="X-UA-Compatible" content="IE=edge">
                    <meta name="viewport" content="width=device-width, initial-scale=1">
                    <title>Hello</title>

                    <!-- Bootstrap CSS -->
                    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

                    <!-- HTML5 Shim and Respond.js IE8 support of HTML5 elements and media queries -->
                    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
                    <!--[if lt IE 9]>
                        <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.3/html5shiv.js"></script>
                        <script src="https://oss.maxcdn.com/libs/respond.js/1.4.2/respond.min.js"></script>
                    <![endif]-->
                </head>
                <body>
                    <h1 class="text-center">Hello from ${cf.country}  ${emoji}</h1>

                    <!-- jQuery -->
                    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
                    <!-- Bootstrap JavaScript -->
                    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
                </body>
            </html>
            `
            }

            export default template
        - check the output with wrangler publish

# Use Location Personalization Features in Cloudflare Workers

- we can also mention the city and state from the user is connecting to, and some more stuffs 
    - changes
        - <h1 class="text-center">Hello from ${cf.city} in ${cf.country}  ${emoji}</h1>

# Deploy to a Custom Domain with Cloudflare Wrangler Environments

- Now copy and paste the zone id from dashboard of your domain to wrangler.toml file
- other thing is Route where we can route any url from that
    - we can also add any route for domain or sub-domain and also with * (wild card).
    - for now add
        - https://staging.greencoin.cf/
    - we can also add a new CNAME record to cloudflare with name and Target-> @ (which is going to point root domain/ alias name)
- now we need to choose between Route & workers_dev
    - because route is a predefined path
    - and workers_dev is a development environment
    - we need to do some changes for preprod and prod in wrangler.toml
        name = "greencoincf"
        type = "webpack"

        account_id = "63aa4349b5d9134c55d699ef7db2a636"
        workers_dev = true
        compatibility_date = "2022-01-24"

        [env.production]
        workers_dev = false
        route = "https://staging.greencoin.cf/"
        zone_id = "0d32a989ca0e8d5e9155163f9f937d2b"
        compatibility_date = "2022-01-24"

    - to publish this 
        -  inside project terminal
            - wrangler publish -e production

    
 
