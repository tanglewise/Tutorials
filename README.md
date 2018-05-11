## Tutorials

Tutorials for interacting with IOTA Tangle

# Other useful commands:

Watching logs:

`journalctl -u iota -f`

Viewing node nelson status:

`curl http://localhost:18600`

Restarting broken nelson:

`sudo curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash - && sudo apt-get install -y nodejs && sudo npm install -g npm && sudo service nelson restart`

Viewing neighbors:

`curl http://localhost:14265 -X POST -H 'X-IOTA-API-Version: 1.4' -H 'Content-Type: application/json' -d '{"command": "getNeighbors"}'`
