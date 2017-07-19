# LeapChat

LeapChat is an ephemeral chat application.  LeapChat uses
[miniLock](https://minilock.io) for challenge/response-based
authentication. This app also enables users to create chat rooms,
invite others to said rooms (via a special URL with a passphrase at
the end of it that is used to generate a miniLock keypair), and of
course send (encrypted) messages to the other chat room participants.


## Security Features

- The server cannot see anyone's usernames

- Users can "leap" from one room to the next so that if an adversary
  clicks on an old invite link, it cannot be used to join the room
  - (Feature coming soon!)

- [Very secure headers](https://securityheaders.io/?q=https%3A%2F%2Fwww.leapchat.org&followRedirects=on)
  thanks to [gosecure](https://github.com/cryptag/gosecure).

- TODO (many more)


## Instances

There is currently one public instance running at
[leapchat.org](https://www.leapchat.org).


# Development / Running

## Dependencies

To install Postgres along with the relevant extensions on Debian-based
Linux distros, run

``` $ bash debian_install.sh ```

On Fedora and friends you can run

```$ bash fedora_install.sh ```

Then, download the latest [PostgREST release](https://github.com/begriffs/postgrest/releases)
and put it in your PATH.


## Install and Run Using Docker and Docker Compose

(If you'd rather not use Docker/Docker Compose, see next section.)

Instead of intalling Postgres and PostgREST you can run it in docker with docker compose.
Make sure you have Docker installed with Docker Compose. Then run:

``` $ docker-compose up ```

This will pull some images from docker-hub and start the following contaiers:
- Postgres at port 5432
- PostgREST at port 3000
- Adminer at port 8081

Adminer is a web UI for managing SQL databases. After the containers
are installed and started, go to `localhost:8081`.

From there you can choose postgres as the database engine and the
login with hostname `postgres`, username and password `superuser` and
database `leapchat`.  In here you can execute the initial scripts for
the database. This you only need to do once.

A folder is created at the projects root called
`_docker-volumes/`. This is where all the data from e.g the postgres
container are placed.  Here the actual database files will be stored.

Once your conatiners are running and you have setup the initial
database scripts you can access postgREST at `localhost:3000`.

If you want to shut down the containers just run:

``` $ docker-compose down ```

If you want to force rebuild of the images just run:

``` $ docker-compose up --build ```

If you want to remove the containers just run:

``` $ docker-compose rm ```


## Install and Run

**Environment variables**

All environment variables that the frontend uses should be declared with standard values for development in `./.env`.
If no specific value are set for a given environment variable then the value in that file will be used.
Right now we have the following environment variables:
 - `BACKEND_URL` This is the url that frontend will use to communicate with the GO backend service.

To install and build static assets:

``` $ npm install ```



To build the frontend run the following:

``` $ npm run dev ```

With the `dev` command, webpack is used to build the frontend and it will automatically rebuild it when you make changes to something in the `./src` directory.

Then, in another terminal, to set up the database and run PostgREST,
which our Go code uses for persistence, run (unless you run it in Docker, see above):

``` $ cd db/ ```

If you're on Linux, now run

``` $ sudo -u postgres bash init_sql.sh ```

On Mac OS X, instead run

``` $ sudo -u $USER bash init_sql.sh ```

(The following commands should be run regardless of whether you're on
Linux or OS X.)

``` $ postgrest postgrest.conf ```

Then, in another terminal session run:

``` $ go build ```

``` $ npm run be ```

Then view <http://localhost:8080>.


## Testing

We use [mocha](https://mochajs.org/) as the testing framework, with [chai](http://chaijs.com/)'s expect API.

To run tests:

``` $ npm test ```

and go tests:

``` $ go test [-v] ./... ```
