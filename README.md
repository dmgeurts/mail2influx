# Mail2Influx

![](./example.png)

A simple mail log data parser and influx data injector. It can parse any log generated by postfix & dovecot, even from giants as Proxmox Mail Gateway and Zimbra.

## Forked - api-key auth

The original code did not do authentication to InfluxDB. So I've added the auth options including api-key based authentication.

-- [dmgeurts](https://github.com/dmgeurts)

## Motivation

Telegraf and other solutions do not fill my needs of mail monitoring, so I put python3 and some regex rules to make this parser, it's given as a GPL 3.0 software, so no warranty in you use case; but you can modify it to fit your needs.

## Installation

You need to install python-influxdb support, search it on the repository or use pip for that.

~~And no, you don't need to create the influxdb database in advance, mail2influx take care of that too, just be sure to name it as your wish in the user variables in the header of the script.~~ Using api-keys requires the database to be created and the api-key to be given write privileges to this database. The code hasn't been altered, but if using an api-key you must create the database and the api-key before configuring the script to use them.

0. Clone this repository to somewhere in you PC `git clone https://github.com/dmgeurts/mail2influx && cd mail2influx` 
1. Copy the "mail2influx.py" script to `/usr/local/bin/`
2. Make it executable `chmod +x /usr/local/bin/mail2influx.py`
3. Edit the `/usr/local/bin/mail2influx.py` file and change:
    - The variable named `influxhost` to your influxdb server ip/name.
    - The variable named `influxdb` if you want to name the influxdb with other name.
    - The variable named `influx_uid` if you want to use uid/pwd auth.
    - The variable named `influx_pwd` if you want to use uid/pwd auth.
    - The variable named `influx_key` if you want to use api-key auth.
4. Copy "mail2influx.service" file to the systemd folder `cp mail2influx.service /lib/systemd/system/`
5. Activate and start the service.
    - `systemctl daemon-reload`
    - `systemctl enable mail2influx`
    - `systemctl start mail2influx`
6. Go to Grafana and create a new dashboard and import the data from one of the files in this repository:
    - The `influx-grafana_dashboard.json` file is tailored to a postfix + dovecot server, and it has a default datasource named **InfluxDB-mail**, you may need to update the influxdb source in grafana to make it work.
    - The `influx-grafana_dashboard-PMG.json` file is tailored for a Proxmox Mail Gateway, and it has a default datasource named **InfluxDB-mailgw**, you may need to update the influxdb source in grafana to make it work.

## Known issues

- ~~No authentication against the influxdb, no need on my env~~
- No ssl/tls against the influxdb, no need on my env
- May contain bugs (code is a work in progress)
- May not be 100% optimized

## About the author

This is free software, see [stdevPavelmc](https://github.com/stdevPavelmc/stdevPavelmc)
