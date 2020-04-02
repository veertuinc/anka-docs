Anka Controller should be listening on port 80 (HTTP). Try pointing your browser to the machine's IP or hostname. You can use `localhost` or `127.0.0.1` if you're on the Controller machine.

Your new dashboard should look like the picture below

![How your new dashboard looks like](/images/getting-started/new-dashboard.png)

### Orientation
Let's take a look at what is now running on your machine:
1. Anka Controller is serving web UI and REST API on port 80.
2. Anka Registry is serving REST API on port 8089.
3. ETCD database server serving on ports 2379 and 2380 (used by Anka Controller).