
# workstation-help

Static HTML/CSS site to host documents for connecting to the AI Institute in Dynamic System's DGX Spark Workstations.

## 1) Install Nginx

SSH into the workstation and run:

```bash
sudo apt update
sudo apt install -y nginx
sudo systemctl enable --now nginx
sudo systemctl status nginx --no-pager
```

Test locally:

```bash
curl -I http://127.0.0.1
```

You should see `HTTP/1.1 200 OK`.

## 2) Create the site directory

For the following commands, replace `workstation` with the name of the workstation e.g. `michelangelo`, `donatello`, `raphael`, or `leonardo`.

```bash
sudo mkdir -p /var/www/workstation-help
sudo chown -R $USER:$USER /var/www/workstation-help
```

Copy the files from this repo into the web root on the server:

```bash
rsync -av --delete ./ /var/www/workstation-help/
```

## 3) Create Nginx site config

Place the provided `workstation-help` Nginx config file at:

```
/etc/nginx/sites-available/workstation-help
```

Change the `IP_ADDRESS` to the static IP address assigned by UW IT. Enable it and disable default:

```bash
sudo ln -s /etc/nginx/sites-available/workstation-help /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```

## 4) Open HTTP in firewall (if UFW is enabled)

```bash
sudo ufw status
sudo ufw allow 'Nginx Full'
```

If UFW is inactive, no action needed.

## 5) Test from another machine

- http://IP_ADDRESS
- http://workstation.meb120.me.washington.edu

If the second one does not resolve off-subnet, ask IT for DNS visibility/routing confirmation.