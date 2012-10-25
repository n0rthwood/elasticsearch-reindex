# Install

> mvn -DskipTests clean package

> sudo $ES_HOME/bin/plugin install karussell/elasticsearch-rollindex

> // or the local one: sudo $ES_HOME/bin/plugin -url ./target -install rollindex

> sudo service elasticsearch restart

you should see 'loaded [rollindex], sites []' in the logs

# Deinstallation

> sudo $ES_HOME/bin/plugin remove rollindex

# Usage

The following command:
> curl -XPUT 'http://localhost:9200/_rollindex?indexPrefix=test&searchIndices=2&rollIndices=3'

creates a new index with 3 aliases: 
 * 'test_feed' points to the latest index and acts as a feeding alias
 * 'test_search' which spans over the last 2 indices
 * 'test_roll' which spans over the last 3 indices, all older indices will be closed

Call the command several times and you get a feeling of what it does.
Then create a cron job which calls it with the periodic you like. But of course you can even
call it manually in aperiodic cycles.

To change the index creation settings just specify them in the request body. For other settings have a look in the source.

# FAQ

 * Why do I'm getting IndexAlreadyExistsException? You roll too often, reduce to per minute at maximum. 
   Or change the pattern to include the seconds.
 * Why is no scheduling including? To keep it simple. Otherwise we would require quartz or similar
 