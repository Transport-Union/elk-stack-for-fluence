Hi, i deployed this to filter and view the logs of the Fleunce i am running on my home server. 

Based on https://github.com/deviantony/docker-elk/tree/main

You need to change passwords following instructions below.

harvesting happens with filebeat/filebeat.docker.yml. Its set to all your running containers. Containerinfo is sent along so you can filter at a later stage. 

Filtering happens with logstash/pipeline/logstash.conf

I run ssh -L 5601:127.0.0.1 <myServer> to view the logs in browser. Elastic -> Analytics -> Discover. Select the logs-* index. 


#### Setting up user authentication

> **Warning**  
> Starting with Elastic v8.0.0, it is no longer possible to run Kibana using the bootstraped privileged `elastic` user.

The _"changeme"_ password set by default for all aforementioned users is **unsecure**. For increased security, we will
reset the passwords of all aforementioned Elasticsearch users to random secrets.

1. Reset passwords for default users

    The commands below reset the passwords of the `elastic`, `logstash_internal` and `kibana_system` users. Take note
    of them.

    ```sh
    docker-compose exec elasticsearch bin/elasticsearch-reset-password --batch --user elastic
    ```

    ```sh
    docker-compose exec elasticsearch bin/elasticsearch-reset-password --batch --user logstash_internal
    ```

    ```sh
    docker-compose exec elasticsearch bin/elasticsearch-reset-password --batch --user kibana_system
    ```

    If the need for it arises (e.g. if you want to [collect monitoring information][ls-monitoring] through Beats and
    other components), feel free to repeat this operation at any time for the rest of the [built-in
    users][builtin-users].

2. Replace usernames and passwords in configuration files

    Replace the password of the `elastic` user inside the `.env` file with the password generated in the previous step.


