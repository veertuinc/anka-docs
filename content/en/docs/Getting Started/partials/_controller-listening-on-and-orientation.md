Anka Controller should be listening on port 80 (http). Try pointing your browser to the machine's ip or host name. You can use `localhost` or `127.0.0.1` when using the machine the server is installed on.

Your new dashboard should look like the picture below

![How your new dashboard looks like](/images/getting-started/new-dashboard.png)

### Orientation
#### Running Applications
Let's take a look on what is now running on your machine:
1. Anka Controller serving web UI and REST API on port 80.
2. Anka Registry serving REST API on port 8089.
3. ETCD database server serving on ports 2379 and 2380 (used by Anka Controller).