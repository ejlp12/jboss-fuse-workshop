# LAB 02 – STARTING THE JBOSS FUSE SERVER FROM THE COMMAND LINE

Open a Terminal window on your machine, and cd to the JBoss Fuse installation directory (e.g.
`~/Servers/jboss-fuse-6.1.0.redhat-379`). From there, execute `bin/fuse` (or `bin\fuse.bat`) to
start JBoss Fuse.

By default, JBoss Fuse will start in interactive mode, which means it both starts the server, and displays the
command Console. There are startup options to refine this, such as `bin/fuse server` to just start the
server without the Console, and `bin/fuse client` to start just the Console that will connect to a locally
running server.

```
Please wait while JBoss Fuse is loading...
100% [========================================================================]

      _ ____                  ______
     | |  _ \                |  ____|
     | | |_) | ___  ___ ___  | |__ _   _ ___  ___
 _   | |  _ < / _ \/ __/ __| |  __| | | / __|/ _ \
| |__| | |_) | (_) \__ \__ \ | |  | |_| \__ \  __/
 \____/|____/ \___/|___/___/ |_|   \__,_|___/\___|

  JBoss Fuse (6.1.0.redhat-379)
  http://www.redhat.com/products/jbossenterprisemiddleware/fuse/

Hit '<tab>' for a list of available commands
and '[cmd] --help' for help on a specific command.

Open a browser to http://localhost:8181 to access the management console

Create a new Fabric via 'fabric:create'
or join an existing Fabric via 'fabric:join [someUrls]'

Hit '<ctrl-d>' or 'osgi:shutdown' to shutdown JBoss Fuse.

JBossFuse:karaf@root> osgi:shutdown -f now
```

At this point, JBoss Fuse is up and running...
But since we're going to be spending the rest of the time in Fuse IDE, let's shutdown this instance and learn
how to run JBoss Fuse from within Fuse IDE. Run the following to shut down JBoss Fuse

```
osgi:shutdown -f now
```
