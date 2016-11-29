# GeeKos - Buildbot

1 - To create the master execute:

```bash
$ buildbot create-master master
```

2 - Change name of file master.cfg.sample to master.cfg

```bash
$ mv master/master.cfg.sample master/master.cfg
```

3 - Now start it:

```bash
$ buildbot start master
```

## Create buildbot slave

1 - To create the slave execute:

```bash
$ buildslave create-slave BASEDIR MASTERHOST:PORT SLAVENAME PASSWORD

$ buildslave create-slave mirror localhost:9989 mirror 11
```

2 - The file located in BASEDIR/info/admin should contain your name and email address. This is the buildslave admin address, and will be visible from the build status page (so you may wish to munge it a bit if address-harvesting spambots are a concern).

3 - The file located in BASEDIR/info/host should be filled with a brief description of the host: OS, version, memory size, CPU speed, versions of relevant libraries installed, and finally the version of the buildbot code which is running the buildslave.

4 - Init Slave

```bash
cd mirror
buildslave start
```

5 - Restart the buildmaster to take the configuration

```bash
$ buildbot restart basedir
```

6 - Config buildslaves

In the master.cfg config the slaves:

```python
c['slaves'].append(BuildSlave("example-slave", "pass"))
```

For the slave created:

```python
c['slaves'].append(BuildSlave("mirror", "11"))
```

### Check BUILDBOT

In your browser check the url:

``` 
localhost:8010
```

## CONFIGURACION BASIC 

To basic config created a lists to each component 
which will be explained

Add in master.cfg:

```python
c = BuildmasterConfig = {}

c['status'] = []

c['slaves'] = []

c['change_source'] = []

c['schedulers'] = []

c['builders'] = []

c['buildbotURL'] = 'http://localhost:8010/'

c['protocols'] = {'pb': {'port': 9989}}

forced_builders = []
```

## Time to use BUILDBOT


When you config buildbot, should be clear about several concepts:

### Change Sources

Which create a Change object each time something is modified in the VC repository. Most ChangeSources listen for messages from a hook script of some sort. Some sources actively poll the repository on a regular basis. All Changes are fed to the Schedulers.

### List to Change Sources

```python
c['change_source'] = []
```

### Schedulers
    
Which decide when builds should be performed. They collect Changes into BuildRequests, which are then queued for delivery to Builders until a buildslave is available.

### List to Schedulers

```python
c['schedulers'] = []
```

### Builders
    
Which control exactly how each build is performed (with a series of BuildSteps, configured in a BuildFactory). Each Build is run on a single buildslave.

### List to Builders

```python
c['builders'] = []
```

## Create Change Sources

Here we do a git-based change_source, which will verify the changes submitted by the repository. The parameters passed are the following:

- Url.
- Name the change.
- Branch repository.
- Time interval for a poll.

Already with this simple configuration we create a change_source.

In this example uses a repo private

Into to master.cfg:

```python
c['change_source'].append(
        GitPoller(repourl="https://myusername:mypassword@github.com/myorgname/myreponame.git"
              project= "geekos-buildbot",
              branches=["master"],
              pollinterval=60)
            )
```

## Create Builder

1 - To create a builder we need some things, which are:

- Name to builder.
- factory with all steps.
- slavename.

### First defined the steps to the build:

Into to master.cfg:

```python
builder = 'geekos-buildbot'

all_steps = [
        ShellCommand(command = 'echo "Hola Geekos"'),
    ]
```

2 - Added the builder

Into to master.cfg:

```python
c['builders'].append(
        BuilderConfig(
            name=builder, 
            factory=BuildFactory(all_steps), 
            slavenames='mirror'
            )
        )
```

## Create Schedulers

Now we create a Scheduler, he will be responsible the collect changes into Build Requests to after delivery to a Builders until a buildslave is available

The parameters are:

- Name.
- Changefilter with name project and branch.
- treeStableTimer.
- builderNames

```python
sbched = SingleBranchScheduler(
        name=builder,
        change_filter = filter.ChangeFilter(project=builder, 
                            branch='master'
                            ),
        treeStableTimer = 10,
        builderNames = [builder])
```

Add Scheduler to all schedulers:

```python
c['schedulers'].append(sbched)
```

Also we can add the option to the builder to that can be forced his construction or execution, for this:


```python
forced_builders.append(builder)
```

And now we created a ForceScheduler that contain all forced builders:

```python
forced_scheduler = ForceScheduler(name='Forzar', builderNames = forced_builders)
```

And we add the force_builders to the schedulers:

```python
c['schedulers'].append(forced_scheduler)
```

## Autentication  

We will create a Object Authz with the configuration to access
the buildbot

```python
authz_cfg=authz.Authz(
    # change any of these to True to enable; see the manual for more
    # options
    auth=auth.BasicAuth([("pyflakes","pyflakes")]),
    gracefulShutdown = False,
    forceBuild = 'auth', # use this to test your slave once it is set up
    forceAllBuilds = True,
    pingBuilder = True,
    stopBuild = True,
    stopAllBuilds = True,
    cancelPendingBuild = True,
)
```

## Web Status

And to finish the tutorial we add to the WebStatus with the configuration
saved in authz_cfg

```python
c['status'].append(html.WebStatus(http_port=8010, authz=authz_cfg))
```

```bash
$ buildbot reconfig master

or

$ buildbot restart master
```

And with this we have a simple configuration a of buildbot to construct a package of debian :DD

#### Sources:

[http://docs.buildbot.net/current/manual/index.html]
[http://docs.buildbot.net/current/tutorial/fiveminutes.html]
